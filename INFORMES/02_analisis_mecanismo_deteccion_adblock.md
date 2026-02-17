# Analisis Tecnico: Mecanismo de Deteccion de Ad Blocker en RTVE Play

**Fecha:** 17 de febrero de 2026
**Complementa a:** `01_informe_tecnico_departamento_IT.md`
**Enfoque:** Ingenieria inversa del codigo de deteccion de ad blocker en THEOplayer y su integracion con RTVE

---

## 1. Resumen

Este informe profundiza en el mecanismo exacto por el cual RTVE Play detecta ad blockers y bloquea la reproduccion de video. El analisis revela que:

- **No se usan tecnicas clasicas de deteccion** (elementos cebo en el DOM, comprobacion de CSS, etc.)
- Se usa una **deteccion indirecta por ausencia de SDK**: THEOplayer comprueba si Google IMA SDK esta disponible en runtime
- RTVE tiene un **fallback deliberado a fichero vacio** (`js2.rtve.es/js/vacio`) que garantiza el disparo de la deteccion
- El error se reporta a **tres sistemas de telemetria** simultaneamente (Conviva, MarkCollector, GTM)
- THEOplayer define un codigo de error especifico: `AD_BLOCKER_DETECTED = 10001`

---

## 2. Codigo fuente de la deteccion

### 2.1 Enumeracion de errores en THEOplayer

Extraido de `pf_video.js`. THEOplayer define errores especificos para ad blocking:

```javascript
// Codigos de error del player:
t[t.AD_ERROR = 10000] = "AD_ERROR"
t[t.AD_BLOCKER_DETECTED = 10001] = "AD_BLOCKER_DETECTED"

// Tipos de error interno (clasificacion):
t.ADBLOCK = "adblock"
t.NETWORK = "network"
t.PARSING = "parsing"
t.PLAYBACK = "playback"
t.GENERAL = "general"

// Identificador de estado:
dK.qLt = "AD_BLOCK_DETECTED"
```

### 2.2 Logica de deteccion: el resolver de integraciones

THEOplayer mantiene un registro de integraciones publicitarias (`google-ima`, `freewheel`, `spotx`, etc.). Cuando necesita reproducir un pre-roll, intenta resolver la integracion. Si el resolver devuelve `null` (porque el SDK de anuncios no se cargo), lanza error `ADBLOCK`.

El fichero `pf_video.js` contiene **tres builds** de THEOplayer con el mismo patron. Codigo desofuscado:

```javascript
// === BUILD 1 ===
i.w6 = function(adConfig, callback, context) {
    try {
        this.E2t = context;
        var parsedSource = RS.Vg.PZ(adConfig);        // Parsear fuente de anuncio
        var handler = this.S2t.o2t(parsedSource.h2t);  // Resolver handler de IMA

        if (!handler) {
            // >>> Si handler es null, se clasifica como ADBLOCK <<<
            throw new eQ(uQ.ADBLOCK, undefined, "Ads could not be loaded.");
        }
        // ... procede con la reproduccion del anuncio
    } catch(error) {
        this._o(r0.y2t(error, adConfig.sources));
    }
}

// === BUILD 2 ===
i.i1 = function(adConfig, callback, context) {
    try {
        this.cQt = context;
        var parsedSource = vI.Lb.OR(adConfig);
        var handler = this.rQt.jUt(parsedSource.zUt);

        if (!handler) {
            throw new yJ(wJ.ADBLOCK);  // Mismo patron
        }
    } catch(error) {
        this.jo(_J.tQt(error, adConfig.sources));
    }
}

// === BUILD 3 (ES module) ===
i.ot = function(adConfig, callback, context) {
    try {
        var parsedSource = pE.Zo.Hx(adConfig);
        var handler = this.a5t.L3t(parsedSource.j3t);

        if (!handler) throw new f6(o6.ADBLOCK);  // Mismo patron
    } catch(error) {
        this.gf(d6.X3t(error, adConfig.sources));
    }
}
```

**Conclusion:** La deteccion se reduce a un simple `if (!handler)`. El `handler` es `null` cuando el IMA SDK no se cargo correctamente.

### 2.3 Deteccion en cascada: tres niveles

#### Nivel 1: SDK no disponible

El bloqueo del IMA SDK loader hace que el resolver devuelva `null`:

```javascript
if (!handler) {
    throw new AdError(ErrorType.ADBLOCK, undefined, "Ads could not be loaded.");
}
```

#### Nivel 2: Respuesta VAST vacia

Incluso si el IMA SDK se carga, si la peticion VAST al ad server falla (ej. `pubads.g.doubleclick.net` bloqueado):

```javascript
if (adSources.length === 0) {
    var message = "Ads could not be loaded.";
    this.dispatcher.error(
        new AdError(ErrorType.ADBLOCK, SubType.FILE_NOT_FOUND),
        message
    );

    if (this.isPreroll) {
        this.playerError = new PlayerError(
            ErrorCode.AD_BLOCKER_DETECTED,  // 10001
            message
        );
        this.dispatchErrorEvent(this.playerError);
    }
}
```

#### Nivel 3: Video publicitario no reproducible

Si el VAST se carga pero el video del anuncio no puede reproducirse:

```javascript
// Variante FreeWheel:
function onAdPlaybackError(event, source) {
    if (this.isPreroll) {
        this.playerError = new PlayerError(
            ErrorCode.AD_BLOCKER_DETECTED,
            "It was not possible to play the ads. Try disabling your ad blocker."
        );
        this.dispatchErrorEvent(this.playerError);
    }
}
```

---

## 3. Mensajes mostrados al usuario

Se han identificado tres mensajes distintos en el codigo:

| Mensaje | Contexto | Disparador |
|---------|----------|------------|
| "Turn off your AdBlocker and try again." | Intento de play sin anuncios | Pre-roll obligatorio no cargado |
| "An ad blocker was detected. Please turn it off and refresh the page in order to watch this video." | Overlay fullscreen de THEOplayer | Error `AD_BLOCK_DETECTED` |
| "It was not possible to play the ads. Try disabling your ad blocker." | Integracion FreeWheel | VAST cargado pero media bloqueada |

Codigo del overlay fullscreen:
```javascript
{
    rv: "AD_BLOCK_DETECTED",
    If: "An ad blocker was detected. Please turn it off and refresh the page in order to watch this video."
}
```

Codigo del mensaje de play bloqueado:
```javascript
r.fQt = function() {
    r.cQt && r.dQt("Starting ads was not possible. Turn off your AdBlocker and try again.");
};
```

---

## 4. El fallback deliberado a fichero vacio

### 4.1 Configuracion del IMA loader

En `pf_video.js`, la configuracion del loader de IMA incluye:

```javascript
'imaLoader': [
    'https://imasdk.googleapis.com/js/sdkloader/ima3',
    'https://js2.rtve.es/js/vacio'  // <<< FALLBACK A FICHERO VACIO
]
```

### 4.2 Implicacion tecnica

Cuando el ad blocker intercepta la peticion a `imasdk.googleapis.com`:
1. El loader intenta el fallback: `js2.rtve.es/js/vacio`
2. Este fichero se carga correctamente (no esta en listas de bloqueo)
3. Pero esta **vacio**: no registra ningun handler de IMA
4. El resolver devuelve `null`
5. THEOplayer clasifica esto como `ADBLOCK`

**Esto es intencional:** RTVE podria haber configurado el fallback para que el video se reproduzca sin anuncios. En su lugar, eligieron un fichero vacio que garantiza la deteccion.

### 4.3 Comparacion con el comportamiento alternativo

THEOplayer soporta una opcion `blockContentIfAdError` que permite elegir:
- `true`: Bloquear contenido si los anuncios fallan (comportamiento actual de RTVE)
- `false`: Continuar al contenido sin anuncios

RTVE ha configurado explicitamente el primer comportamiento.

---

## 5. Tratamiento en el lado RTVE (capa de aplicacion)

### 5.1 Handler de errores de anuncio

```javascript
rtvePlayerService.handleAdError(function(e) {
    logger.logDebug('adevt aderror');
    $this.sendData('ADERROR_THEO');  // Envia evento a GTM dataLayer
});
```

### 5.2 Handler de errores generales del player

```javascript
rtvePlayerService.handleError(function(event) {
    var errorResponse = rtvePlayerService.toPlayerErrorResponse({ event, idAsset });
    rtvePlayerService.showError(errorResponse.code, capa);
});
```

### 5.3 Funcion showError

```javascript
rtvePlayerService.showError = function(code, capa) {
    var urlCode = "/errores/errorGenerico.shtml";
    switch (code) {
        case 402: urlCode = "/errores/error402.shtml"; break;  // Geo-restringido
        case 409: urlCode = "/errores/error409.shtml"; break;  // Conflicto
    }
    var url = "//" + domain + urlCode;
    $.ajax({
        url: url,
        success: function(response) {
            capa.find('.vjs-error-display').html(response).show();
        }
    });
};
```

El error `AD_BLOCKER_DETECTED` (10001) no tiene tratamiento especifico en el switch, por lo que muestra `errorGenerico.shtml`. Pero THEOplayer ya ha mostrado su propio overlay antes de que este codigo se ejecute.

---

## 6. Telemetria: como RTVE mide el uso de ad blockers

### 6.1 Conviva: estado AD_BLOCKED

THEOplayer reporta a Conviva un estado especifico:

```javascript
function onAdStateChange(event) {
    if (event.errorType === ErrorType.ADBLOCK) {
        this.adState = this.AdStates.AD_BLOCKED;
    } else {
        this.adState = this.AdStates.AD_ERROR;
    }
}
```

Estados del ciclo de vida de anuncios en Conviva:
- `AD_IMPRESSION` - Anuncio visto
- `AD_STARTED` - Anuncio iniciado
- `AD_COMPLETED` - Anuncio completado
- `AD_BLOCKED` - Ad blocker detectado
- `AD_ERROR` - Error generico
- `AD_SKIPPED` - Anuncio saltado

Esto permite dashboards en tiempo real con el porcentaje de usuarios con ad blocker.

### 6.2 MarkCollector (sistema propio RTVE)

`mark-collector.iife.js` define un evento predefinido:

```javascript
{
    activeTagId: "16",
    tagName: "Adblock",
    eventType: "Triggered",
    deviceType: { deviceName: "All" },
    application: { applicationName: "All" },
    event: { eventId: "16", eventName: "Adblock" }
}
```

### 6.3 Google Tag Manager (dataLayer)

El error se empuja al dataLayer de GTM como `ADERROR_THEO`, lo que permite disparar tags adicionales (ej. enviar a Google Analytics, Adobe Analytics, etc.).

---

## 7. Parametros del Ad Tag: datos del usuario enviados a Google

La funcion `processAdUrlTemplate` construye la URL del anuncio con estos parametros:

| Parametro | Contenido | Implicacion |
|-----------|-----------|-------------|
| `WIDTH` / `HEIGHT` | Dimensiones de pantalla | Segmentacion por dispositivo |
| `USER_AGENT` | User-Agent del navegador | Fingerprinting |
| `CONSENT` | String TCF de consentimiento GDPR | Que datos pueden usar los ad networks |
| `GDPR` | Flag de aplicabilidad | Siempre "1" para usuarios EU |
| `PLAY_PLUS` | "0" o "1" | Si el usuario tiene suscripcion de pago |
| `CACHEBUSTER` | Timestamp | Anti-cache |
| `SEGUIR_VIENDO` | Flag | Live vs. resume |

El parametro `PLAY_PLUS` es relevante: **usuarios de pago pueden recibir menos anuncios**, incentivando la suscripcion.

---

## 8. El fichero bridge3.741.1_es.html (Google IMA SDK)

### 8.1 Que es

Es el iframe bridge del Google IMA SDK v3.741.1 (localizacion en espanol). Se carga desde `imasdk.googleapis.com/js/core/bridge3.741.1_es.html`.

### 8.2 Que NO es

- **No contiene deteccion de ad blocker.** El IMA SDK simplemente intenta servir anuncios; si falla, dispara un evento de error.
- El parametro `ad_block` en lineas 257 y 1041 es un **parametro de DFP/DoubleClick** para numeracion de companion ad slots, no deteccion de bloqueo.
- El parametro `bait` en linea 748 es un **tag de bitrate/audio** del media stream, no un elemento cebo.

### 8.3 CSS inyectado por el ad blocker del usuario

Las lineas 1533-1534 del fichero guardado contienen CSS inyectado por la **propia extension de ad blocker del usuario** (cosmetic filters de EasyList). Esto aparecio al guardar la pagina completa y no forma parte del IMA SDK. Selectores como `:root adblocker { display: none !important; }` son reglas del ad blocker, no de RTVE.

---

## 9. Diagrama completo del flujo de deteccion

```
Usuario visita pagina de video RTVE
        |
        v
init_pfvideo.js: comprueba disponibilidad
  - publicado? geo-permitido? contenido de pago?
        |
        v
pf_video (THEOplayer) se inicializa
        |
        v
loadAds(): solicita config de anuncios a API RTVE
  promo.getImaConfig(apidomain, idAsset, promo)
        |
        v
addAdSourceConfiguration({
    integration: 'google-ima',
    sources: adUrl
})
        |
        v
THEOplayer intenta cargar IMA SDK
  Primario: imasdk.googleapis.com/js/sdkloader/ima3
  Fallback: js2.rtve.es/js/vacio (FICHERO VACIO)
        |
        +---[Ad blocker bloquea IMA SDK]--> Se carga 'js/vacio'
        |                                        |
        |                                        v
        |                                  Handler = null
        |                                        |
        |                                        v
        |                                  throw AdError(ADBLOCK)
        |                                  "Ads could not be loaded."
        |                                        |
        v                                        |
IMA SDK se carga OK                              |
        |                                        |
        v                                        |
VAST request a ad server                         |
  (pubads.g.doubleclick.net)                     |
        |                                        |
        +---[Ad blocker bloquea VAST]-----> adSources.length === 0
        |                                        |
        |                                        v
        |                                  AdError(ADBLOCK, FILE_NOT_FOUND)
        |                                  PlayerError(10001)
        |                                        |
        v                                        |
VAST se carga OK                                 |
        |                                        |
        v                                        |
Reproducir video publicitario                    |
        |                                        |
        +---[Ad blocker bloquea media]----> AdError(AD_BLOCKER_DETECTED)
        |                                  "Try disabling your ad blocker"
        |                                        |
        v                                        |
Anuncio se reproduce OK                          |
        |                                        v
        v                               BLOQUEO DE REPRODUCCION
Video de contenido                       Overlay: "Turn off your AdBlocker"
se reproduce                                     |
                                          +------+------+------+
                                          |             |      |
                                          v             v      v
                                     Conviva       MarkCol.  GTM
                                    AD_BLOCKED     tag 16   ADERROR_THEO
```

---

## 10. Aspectos cuestionables

### 10.1 Clasificacion abusiva de errores

THEOplayer clasifica **cualquier fallo en el pipeline publicitario** como `ADBLOCK`. Esto incluye:
- Errores de red genuinos (servidor de anuncios caido temporalmente)
- Timeouts por conexion lenta
- Respuestas VAST malformadas del ad server
- Problemas de CORS o certificados SSL

Un usuario con mala conexion puede recibir el mensaje "Turn off your AdBlocker" cuando no tiene ningun ad blocker instalado.

### 10.2 Pre-roll obligatorio sin fallback a contenido

RTVE ha elegido configurar THEOplayer con `blockContentIfAdError = true`. Esto significa que si el pre-roll falla por cualquier razon, **el contenido publico nunca se reproduce**. No existe degradacion graciosa (graceful degradation).

### 10.3 Fallback a fichero vacio como decision de diseno

La configuracion `['imasdk.googleapis.com/...', 'js2.rtve.es/js/vacio']` demuestra decision deliberada. Alternativas que RTVE descarto:
- Fallback a reproduccion sin anuncios
- Fallback a anuncios propios (no dependientes de terceros)
- Fallback con mensaje informativo y opcion de continuar

### 10.4 Triple telemetria del estado de ad blocker

La deteccion se reporta a tres sistemas simultaneamente (Conviva, MarkCollector, GTM), lo que indica que el dato tiene alto valor interno. Esto sugiere que RTVE monitoriza activamente el porcentaje de usuarios con ad blocker y toma decisiones de negocio basadas en estos datos.

### 10.5 Contenido publico condicionado a publicidad privada

RTVE es una corporacion publica financiada con presupuestos generales del Estado. La decision tecnica de condicionar el acceso al contenido a la carga exitosa de publicidad servida por Google (con servidores fuera de la UE) plantea cuestiones de:
- Derecho de acceso a contenido financiado publicamente
- Cesion de datos de ciudadanos a plataformas publicitarias extranjeras
- Dependencia tecnologica de infraestructura privada para un servicio publico

---

## 11. Resumen tecnico

| Aspecto | Detalle |
|---------|---------|
| Tipo de deteccion | Indirecta: ausencia de SDK de anuncios en runtime |
| Codigo de error | `AD_BLOCKER_DETECTED` (10001) |
| Tipo de error interno | `ADBLOCK` (string) |
| Numero de niveles de deteccion | 3 (SDK, VAST, media) |
| Fallback configurado | `js2.rtve.es/js/vacio` (fichero vacio deliberado) |
| Bloqueo de contenido | Si, pre-roll obligatorio sin fallback |
| Mensajes al usuario | 3 variantes en ingles |
| Sistemas de telemetria | Conviva (AD_BLOCKED), MarkCollector (tag 16), GTM (ADERROR_THEO) |
| Posibilidad de bypass simple | No, la deteccion esta integrada en el pipeline de reproduccion |
| Fichero principal | `pf_video.js` (THEOplayer 9.4.1, 7.7 MB) |

---

*Fin del analisis tecnico.*
