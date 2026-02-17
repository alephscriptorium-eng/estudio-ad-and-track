# Informe Técnico: Análisis de la Arquitectura de Tracking y Reproducción de Vídeo en RTVE Play

**Fecha:** 17 de febrero de 2026  
**Clasificación:** Interno — Departamento Técnico  
**Autor:** [Departamento de Desarrollo]  
**Versión:** 1.0  

---

## 1. Resumen Ejecutivo

Se ha realizado un análisis forense de la carga de red (HAR) y del código fuente JavaScript de la página de reproducción de vídeo de RTVE Play (`https://www.rtve.es/play/videos/la-revuelta/programa-22-01-26-oliver-laxe-angeles-toledano/16906368/`). El objetivo era determinar por qué la reproducción de vídeo falla cuando el usuario tiene un bloqueador de publicidad activo en el navegador.

**Conclusión principal:** La arquitectura de RTVE Play acopla la inicialización de la publicidad (Google IMA SDK) y múltiples servicios de tracking/analytics como requisito previo a la resolución del stream de vídeo. Cuando un ad blocker interrumpe estos servicios, la cadena de inicialización se rompe y el vídeo nunca llega a cargarse, sin mostrar un mensaje de error claro al usuario.

---

## 2. Metodología

- **Captura HAR** del tráfico de red completo de la página (157 requests, 18.2 MB).
- **Análisis estático** del código fuente descargado (~44 archivos JavaScript y CSS).
- **Herramientas:** Python 3 para parsing del HAR, grep/análisis manual del código JS.
- **Entorno:** Navegador con extensión de bloqueo de publicidad estándar activa.

---

## 3. Inventario de Servicios de Terceros Detectados

### 3.1 Tabla resumen

| Servicio | Dominio(s) | Requests | Bloqueadas | Función | Impacto en reproducción |
|----------|-----------|----------|------------|---------|------------------------|
| **Conviva** | `*.appgw.conviva.com` | 24 | 0 | Telemetría de vídeo | CRÍTICO — intercepta XHR/fetch/WebSocket |
| **VWO** | `dev.visualwebsiteoptimizer.com` | 15 | 8 | A/B testing | Medio — carga scripts auxiliares |
| **Google IMA SDK** | `imasdk.googleapis.com` | 4 | 1 | SDK de publicidad en vídeo | CRÍTICO — prerequisito del player |
| **Adobe Audience Manager** | `dpm.demdex.net` | 5 | 5 | Perfilado de audiencia | Bajo directo, alto en privacidad |
| **Gigya (SAP)** | `cdns.eu1.gigya.com`, `users.rtve.es` | 6 | 0 | Autenticación/login | Medio — dependencia del player |
| **Chartbeat** | `static.chartbeat.com` | 2 | 2 | Analítica en tiempo real | Bajo |
| **OneTrust** | `cdn.cookielaw.org` | 3 | 1 | Gestión de consentimiento de cookies | Medio |
| **Google Tag Manager** | `www.googletagmanager.com` | 1 | 0 | Gestión de tags | Alto — orquesta carga de Conviva |
| **Adobe Analytics** | `sadbmetrics.rtve.es`, `js2.rtve.es/js/omnniture/` | 5 | 0 | Medición de audiencia | Bajo |
| **Pixel tracker** | `tvee.spxl.socy.es` | 1 | 1 | Tracking personalizado | Bajo |
| **Adobe Target** | `mboxedge37.tt.omtrdc.net` | 1 | 0 | Personalización/testing | Bajo |

### 3.2 Servicios propios de RTVE (no tracking)

| Servicio | Dominio | Requests | Función |
|----------|---------|----------|---------|
| API de vídeo | `api.rtve.es` | 1 | Metadatos del vídeo |
| API de estadísticas | `api2.rtve.es` | 1 | Stats para Conviva/Adobe |
| Servicios de usuario | `secure2.rtve.es` | 3 | Favoritos, historial, notificaciones |
| Assets estáticos | `js.rtve.es`, `js2.rtve.es`, `css.rtve.es`, `css2.rtve.es`, `img.rtve.es` | 59 | JS, CSS, imágenes |

---

## 4. Análisis de la Cadena de Inicialización del Player

### 4.1 Flujo de carga (orden cronológico del HAR)

```
Request #   Recurso                              Status   Dependencia
────────────────────────────────────────────────────────────────────────
#7          require.js                           304 OK   → Módulo loader
#48         config_video.js                      304 OK   → Config RequireJS
                                                           Define: init_pfvideo depende de gigyaManager
#50         gigyaManager.js                      304 OK   → Autenticación
#51         jquery.js                            304 OK   → Librería DOM
#62         gigya.js (626 KB)                    304 OK   → SDK Gigya completo
#65         init_pfvideo.js                      304 OK   → Orquestador de init
#67         pf_video.js (redirect 302)           302      → Redirect a js2.rtve.es
#69         pf_video.js (7.7 MB)                 304 OK   → THEOplayer + lógica RTVE
#82         ima3.js (466 KB)                     200 OK   → Google IMA SDK publicidad
#89         api.rtve.es/api/videos/16906368.json 200 OK   → Metadatos del vídeo ✓
#93-95      bridge3.741.1_es.html (IMA bridge)   200/0/200 → ⚠️ Request #94 BLOQUEADA
#97         stats.json                           200 OK   → Datos para Conviva/Adobe
#101        convivaAppTracker-v1_4_3.js          304 OK   → Conviva cargado por GTM
────────────────────────────────────────────────────────────────────────
             ❌ AQUÍ SE ROMPE LA CADENA
             No se realiza ninguna petición a ztnr.rtve.es para resolver el stream
────────────────────────────────────────────────────────────────────────
#128-156    24x POST a conviva.com/ctp           200 OK   → Telemetría sobre vídeo NO reproducido
```

### 4.2 El problema: el stream nunca se solicita

La API de vídeo (request #89) responde correctamente con las URLs de streaming:

```
https://ztnr.rtve.es/ztnr/res/_PROVIDER_/video/hd_full/_TOKEN_    (1080p, 4.3 Mbps)
https://ztnr.rtve.es/ztnr/res/_PROVIDER_/video/hd_ready/_TOKEN_   (720p, 3.0 Mbps)
https://ztnr.rtve.es/ztnr/res/_PROVIDER_/video/hq/_TOKEN_         (576p, 2.0 Mbps)
https://ztnr.rtve.es/ztnr/res/_PROVIDER_/video/high/_TOKEN_       (360p, 1.2 Mbps)
```

Sin embargo, **ninguna petición a `ztnr.rtve.es` aparece en el HAR**. La resolución de `_TOKEN_` y `_PROVIDER_` nunca se ejecuta porque la cadena de inicialización se interrumpe antes.

### 4.3 Punto exacto de ruptura

Analizando el código de `pf_video.js` (THEOplayer):

1. `init_pfvideo.js` → función `n()` llama a `pf_video.init(capa, dataAsset, ztnrdomain, apidomain)`
2. `pf_video.init()` configura `mediaSelector.init(ztnrdomain, src)`
3. El player intenta crear el `AdPlayer` vía IMA SDK → `createAdPlayer()` → `bridge3.741.1_es.html`
4. **La request #94 al IMA bridge iframe se bloquea** por el ad blocker (status 0)
5. El player no continúa al paso de `mediaSelector.getMedia()` → `resolveNoDrm()` / `resolveDrm()`
6. `resolveNoDrm()` haría `$.get(ztnrdomain + "/ztnr/movil/thumbnail/rtveplayw/videos/" + idAsset + ".png?q=v2")` → pero **nunca se ejecuta**

**No existe un mecanismo de fallback** que permita reproducir el vídeo si la inicialización del Ad Player falla.

---

## 5. Análisis de Conviva AppTracker v1.4.3

### 5.1 Interceptación de protocolos de red

El archivo `convivaAppTracker-v1_4_3.js` (423 KB) realiza **monkey-patching** de los tres protocolos de comunicación del navegador:

```javascript
// Dentro de Ed.prototype.decorateXMLHttpRequest():
XMLHttpRequest.prototype.open = function() { /* wrapper de Conviva */ }
XMLHttpRequest.prototype.send = function() { /* wrapper de Conviva */ }
XMLHttpRequest.prototype.setRequestHeader = function() { /* wrapper de Conviva */ }

// Dentro de Ed.prototype.decorateFetch():
window.fetch = this.interceptFetch.bind(this);

// Dentro de Ed.prototype.decorateWebSocket():
WebSocket.prototype.send = function(n) { /* wrapper de Conviva */ }
window.WebSocket = function(t, n) { /* constructor reemplazado */ }
```

### 5.2 Alcance de la interceptación

Conviva no solo monitoriza las peticiones del player de vídeo. Su configuración incluye:

- **`networkRequestTrackingConfiguration`**: tracking de todas las peticiones HTTP/fetch
- **`exceptionAutotracking`**: captura de todas las excepciones JavaScript de la página
- **`lifecycleAutotracking`**: eventos de ciclo de vida del navegador
- **`clickcc`**: tracking de clics del usuario con selector CSS amplio
- **`sessionReplayConfiguration`**: capacidad de grabación de sesión completa del usuario (screen recording vía `rrweb`)
- **Configuración remota**: Conviva descarga su configuración desde `rc.conviva.com` y la almacena en `localStorage`

### 5.3 Datos del session recording

El código de Conviva incluye un **Web Worker completo para grabación de sesión** que:
- Almacena eventos en IndexedDB (`conviva-session-record-db`)
- Comprime con `pako` (gzip) y sube a Google Cloud Storage
- Incluye: movimiento de ratón, clics, scroll, cambios de input, mutaciones del DOM
- Se activa según la configuración remota, no localmente

### 5.4 Telemetría sin reproducción

Los 24 POSTs a `conviva.com/ctp` envían datos binarios comprimidos. Respuesta del servidor: `"ok"`. Estos heartbeats se disparan **aunque el vídeo nunca se reproduzca**, reportando:

```json
{
  "c3.cm.brand": "RTVE",
  "c3.cm.channel": "la1",
  "c3.cm.seriesName": "La Revuelta",
  "c3.cm.id": "16906368",
  "Conviva.streamType": "VOD",
  "ageRange": "No recomendable para menores de 12 años",
  "sgce": "P611PGS00703228",
  "progId": "1000646"
}
```

### 5.5 Carga vía GTM

Conviva no se carga directamente desde el player, sino a través de Google Tag Manager:

```
Iniciador de convivaAppTracker-v1_4_3.js:
  → gtm.js:738 → función TR → función SR
```

Esto implica que la **lógica de tracking es gestionada por el equipo de marketing/analytics**, no por el equipo del player de vídeo.

---

## 6. Google IMA SDK — Publicidad como requisito

### 6.1 Integración en THEOplayer

El player `pf_video.js` es **THEOplayer 9.4.1** (un player comercial). La integración de IMA incluye:

- Carga del SDK: `ima3.js` (466 KB)
- Creación del Ad Player: `createAdPlayer()` en línea 6337
- Bridge iframe: `bridge3.741.1_es.html` — necesario para la comunicación cross-origin del IMA SDK
- La inicialización del ad player es **síncrona y bloqueante** respecto al flujo de carga del vídeo

### 6.2 Requests bloqueadas del IMA

```
#93  bridge3.741.1_es.html → 200 OK (0 bytes)
#94  bridge3.741.1_es.html → STATUS 0 (BLOQUEADA)     ← ⚠️ Punto de fallo
#95  bridge3.741.1_es.html → 200 OK (927 KB)
```

El ad blocker bloquea una de las tres peticiones al bridge, lo que es suficiente para que la inicialización del ad player no se complete correctamente.

---

## 7. Requests Bloqueadas — Detalle completo

| # | Dominio | Path | Función |
|---|---------|------|---------|
| 58 | `dev.visualwebsiteoptimizer.com` | `/eu01/v.gif` | VWO pixel de tracking |
| 72-73 | `dev.visualwebsiteoptimizer.com` | `/cdn/analysis/4.0/opaEv-*.js` | VWO análisis |
| 74 | `dev.visualwebsiteoptimizer.com` | `/ee.gif` | VWO evento |
| 76-78 | `dev.visualwebsiteoptimizer.com` | (repeticiones) | VWO reintentos |
| 94 | `imasdk.googleapis.com` | `/js/core/bridge3.741.1_es.html` | **IMA bridge — CRÍTICO** |
| 105 | `tvee.spxl.socy.es` | `/pixel/js/TVEE_738_746_736/...` | Pixel TVEE |
| 106 | `static.chartbeat.com` | `/js/chartbeat.js` | Chartbeat analytics |
| 107 | `static.chartbeat.com` | `/js/chartbeat_mab.js` | Chartbeat MAB |
| 115, 125, 127, 129, 151 | `dpm.demdex.net` | `/id` | Adobe Audience Manager |
| 119 | `cdn.cookielaw.org` | `/scripttemplates/.../otBannerSdk.js` | OneTrust banner |

---

## 8. Diagrama de Dependencias del Player

```
                        ┌─────────────────┐
                        │  config_video.js │
                        │  (RequireJS)     │
                        └────────┬────────┘
                                 │
                    ┌────────────┴────────────┐
                    │                         │
              ┌─────▼──────┐          ┌───────▼───────┐
              │gigyaManager│          │ init_pfvideo  │
              │  (auth)    │          │ (orquestador) │
              └─────┬──────┘          └───────┬───────┘
                    │                         │
                    │              ┌──────────┴──────────┐
                    │              │                      │
              ┌─────▼──────┐  ┌───▼────┐         ┌──────▼──────┐
              │  gigya.js   │  │jQuery  │         │  pf_video   │
              │  (626 KB)   │  │        │         │ THEOplayer  │
              └─────┬──────┘  └────────┘         │  (7.7 MB)   │
                    │                             └──────┬──────┘
                    │                                    │
              ┌─────▼──────┐              ┌──────────────┼──────────────┐
              │Gigya CDN   │              │              │              │
              │(SSO, SDK)  │        ┌─────▼──────┐ ┌────▼─────┐ ┌─────▼──────┐
              └────────────┘        │  IMA SDK   │ │MediaSel. │ │  Conviva   │
                                    │ (ads)      │ │(stream)  │ │ (GTM)      │
                                    └─────┬──────┘ └────┬─────┘ └─────┬──────┘
                                          │             │             │
                                    ┌─────▼──────┐     │       ┌─────▼──────┐
                                    │IMA Bridge  │     │       │ XHR/fetch  │
                                    │(iframe)    │     │       │ intercept  │
                                    └─────┬──────┘     │       └────────────┘
                                          │             │
                                     ❌ BLOQUEADO       │
                                          │             │
                                          ╳     ┌──────▼──────┐
                                     (no llega) │ ztnr.rtve.es│ ← NUNCA SE EJECUTA
                                                │ (stream)    │
                                                └─────────────┘
```

---

## 9. Recomendaciones Técnicas

### 9.1 Severidad Alta

1. **Desacoplar la inicialización del Ad Player de la carga del stream.** El player debe poder reproducir contenido aunque el IMA SDK falle. Implementar un `try-catch` con fallback en `createAdPlayer()` de THEOplayer.

2. **Eliminar la interceptación global de XHR/fetch/WebSocket por Conviva.** Configurar Conviva para que solo monitorice las peticiones del player de vídeo, no todas las peticiones de la página. Revisar `networkRequestTrackingConfiguration.blocklist` y cambiarlo de `["*"]` (bloquear todo excepto Conviva) a una lista explícita.

3. **Mover Conviva fuera de GTM.** La gestión de un componente que intercepta toda la comunicación de red no debería estar controlada por una herramienta de marketing. Cargarlo directamente desde el player si es tecnológicamente necesario.

### 9.2 Severidad Media

4. **Auditar la configuración de session replay de Conviva.** El código incluye capacidad completa de grabación de sesión (rrweb). Verificar si `sessionReplayConfiguration` está activa en producción y cuál es la cobertura de usuarios.

5. **Implementar degradación graciosa (graceful degradation).** Si servicios de tracking fallan, el vídeo debe seguir reproduciéndose. Añadir timeouts y fallbacks en todos los puntos de dependencia.

6. **Reducir la carga de JavaScript.** El total de JS cargado es excesivo:
   - `pf_video.js` (THEOplayer): 7.7 MB
   - `gigya.js`: 626 KB
   - `ima3.js`: 466 KB
   - `convivaAppTracker`: 423 KB
   - `init_pfvideo.js`: 175 KB
   - `jquery.js`: 94 KB
   - Total: ~9.5 MB de JavaScript solo para reproducir un vídeo

### 9.3 Severidad Baja

7. **Eliminar Adobe Audience Manager (demdex.net)** si no es estrictamente necesario para un servicio público.

8. **Evaluar la necesidad de VWO (Visual Website Optimizer)** con 15 requests para A/B testing en una página de reproducción de vídeo.

9. **Unificar las peticiones de autenticación Gigya.** Se observan dos API keys distintas y múltiples llamadas a `sdk.config.get`.

---

## 10. Datos Técnicos de Referencia

### 10.1 Versiones detectadas

| Componente | Versión |
|-----------|---------|
| THEOplayer | 9.4.1 |
| Conviva AppTracker | 1.4.3 |
| Google IMA SDK | 3 (bridge 741.1) |
| jQuery | 1.12.4 (+ versión bundled en pf_video) |
| RequireJS | (bundled en pages/pf-programas-tve/1.43.3) |
| Gigya SDK | EU1 |
| GTM Container | GTM-XGJJ |
| Adobe AppMeasurement | JS-2.17.0 |

### 10.2 API keys/IDs expuestos

| Servicio | Identificador |
|----------|--------------|
| Gigya API Key | `3_MVrnwOzwUbykkDXOuSGkJfyMFEpG8k4MI3A66RsIKg91icSRNMZ4XnfzYXsU6cmm` |
| Gigya API Key (secundaria) | `3_xpC-b4gZO0jIm0DrLJUPp05bL-fjbLPeNZ9t8wTTQpB2DJyf2nz2ksLmTwl8LJzW` |
| GTM Container | `GTM-XGJJ` |
| Adobe Org ID | `0D3C2C1653DB264C0A490D4B@AdobeOrg` |
| VWO Account | `864486` |
| OneTrust Domain | `1ce517c9-60a0-49ff-8d5d-a8d756cdb3cf` |
| Conviva Customer Key | Hash SHA: `fda447e9ebc2e42ee456a10f8aba4f81b6ff14e5` |
| Conviva Endpoint | `https://appgw.conviva.com` |
| RTVE Video ID | `16906368` |
| RTVE Program ID | `1000646` |

### 10.3 Ficheros de evidencia

- `www.rtve.es.har` — Captura HAR completa (18.2 MB, 157 requests)
- `TVE_files/` — 44 archivos de código fuente descargados de la página

---

*Fin del informe técnico.*
