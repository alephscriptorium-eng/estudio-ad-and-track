# Informe para el Defensor del Espectador de RTVE

## Análisis del Servicio de Reproducción de Vídeo en RTVE Play: Prácticas de Vigilancia Digital y Denegación de Servicio Público

---

**Fecha:** 17 de febrero de 2026  
**Asunto:** Queja formal sobre las prácticas de tracking comercial y la arquitectura de reproducción de vídeo en rtve.es/play  
**Contenido analizado:** *La Revuelta*, episodio del 26/01/2026 — Oliver Laxe y Ángeles Toledano  
**URL:** `https://www.rtve.es/play/videos/la-revuelta/programa-22-01-26-oliver-laxe-angeles-toledano/16906368/`  

---

## 1. Objeto de la Queja

El presente informe documenta y evidencia que la plataforma de vídeo de Radiotelevisión Española (RTVE Play) presenta dos problemas graves desde la perspectiva del ciudadano y su derecho de acceso al servicio público audiovisual:

1. **Denegación de servicio:** La reproducción de vídeo se impide completamente cuando el navegador del usuario tiene activa una extensión de bloqueo de publicidad, sin ofrecer alternativa ni mensaje explicativo alguno.

2. **Uso extensivo de servicios de vigilancia digital comercial:** Para acceder a un contenido de servicio público ya financiado con impuestos, el ciudadano se ve sometido, sin conocimiento ni consentimiento informado efectivo, a un ecosistema de rastreo que incluye grabación de sesión, perfilado de audiencia, A/B testing comercial e interceptación de todas las comunicaciones de red del navegador.

---

## 2. Contexto: RTVE como Servicio Público

RTVE es la corporación de comunicación audiovisual de titularidad estatal española. Su financiación proviene fundamentalmente de los Presupuestos Generales del Estado, es decir, de los impuestos de los ciudadanos. Según la Ley 17/2006 de la radio y la televisión de titularidad estatal, RTVE tiene la obligación de:

- Garantizar el acceso universal a su servicio.
- Promover los derechos fundamentales, incluyendo el derecho a la privacidad.
- Actuar con independencia respecto a intereses comerciales.

El mandato-marco de RTVE establece que el servicio digital debe ser accesible y no debe supeditar el acceso a contenido público a la aceptación de prácticas comerciales de terceros.

---

## 3. Evidencias Documentadas

### 3.1 El vídeo no se reproduce: evidencia cuantitativa

Se ha realizado una captura completa del tráfico de red (fichero HAR) al intentar reproducir un episodio de *La Revuelta*. Los datos son inequívocos:

| Dato | Valor |
|------|-------|
| Total de peticiones de red realizadas por la página | **157** |
| Peticiones bloqueadas por el navegador | 17 |
| Peticiones al servidor de streaming de vídeo (`ztnr.rtve.es`) | **0 (cero)** |
| Peticiones de tracking y telemetría de terceros | **68** (43% del total) |

**El resultado es que el vídeo nunca se carga.** La página realiza 157 peticiones de red, pero ni una sola de ellas es para obtener el contenido de vídeo que el ciudadano desea ver. El servidor de streaming de RTVE (`ztnr.rtve.es`) no recibe ninguna petición.

Se evidencia que la arquitectura web de RTVE condiciona la reproducción de contenido público a la carga previa de servicios publicitarios y de rastreo.

### 3.2 Servicios de terceros a los que se expone al usuario

Cuando un ciudadano visita RTVE Play para ver un programa de televisión pública, su navegador se comunica con los siguientes servicios comerciales de terceros:

#### a) **Conviva** (empresa estadounidense de analítica de vídeo) — 24 peticiones

Conviva es la entidad con mayor tráfico de terceros en la página. Su software realiza las siguientes acciones en el navegador del ciudadano:

- **Intercepta todas las comunicaciones de red del navegador:** El código de Conviva reemplaza las funciones nativas del navegador para enviar peticiones HTTP (`XMLHttpRequest`), para la API moderna de red (`fetch`) y para conexiones en tiempo real (`WebSocket`). Esto significa que **toda comunicación que la página realice pasa a través del código de Conviva**, no solo la relativa al vídeo.

- **Capacidad de grabación de sesión:** El código incluye un módulo de *session replay* basado en la biblioteca `rrweb` que puede grabar: movimientos del ratón, clics del usuario, contenido del texto escrito en formularios, desplazamiento de la página (scroll), y todas las modificaciones visuales de la interfaz. Estos datos se almacenan localmente en el navegador (IndexedDB) y se envían a servidores de Google Cloud Storage.

- **Envía 24 paquetes de datos sobre un vídeo que no se reproduce.** Incluso cuando el vídeo no llega a cargarse, Conviva envía telemetría que incluye: nombre del programa (*La Revuelta*), cadena (*La 1*), clasificación por edad (*No recomendable para menores de 12 años*), identificador del programa, y tipo de contenido (*VOD*).

- **Configuración remota:** Conviva descarga su configuración desde sus propios servidores (`rc.conviva.com`), lo que significa que el comportamiento del tracking puede cambiar sin que RTVE modifique su página web.

#### b) **Adobe Audience Manager / Demdex** (empresa estadounidense) — 5 peticiones (todas bloqueadas)

Adobe Audience Manager es un servicio de **perfilado comercial de audiencia**. Su dominio (`demdex.net`) es ampliamente conocido como servicio de tracking invasivo. Su función es crear un perfil del usuario que puede compartirse entre múltiples anunciantes y plataformas.

Un servicio público de televisión no debería utilizar un servicio de perfilado comercial de audiencia.

#### c) **VWO — Visual Website Optimizer** (empresa india) — 15 peticiones

VWO es una herramienta de **A/B testing y optimización de conversión** diseñada para comercio electrónico. Se utiliza para probar diferentes versiones de una página web con distintos segmentos de usuarios. En el contexto de un servicio público de televisión, la «conversión» no debería ser un objetivo, ya que no se está vendiendo un producto.

La cuenta de VWO utilizada es la número **864486**.

#### d) **Google IMA SDK** (Google, empresa estadounidense) — 4 peticiones

El SDK de publicidad de Google (Interactive Media Ads) es el componente cuyo bloqueo causa directamente la interrupción de la reproducción. Su función es insertar publicidad en vídeo. **La arquitectura de RTVE impide ver el vídeo si este componente publicitario no se carga correctamente.**

#### e) **Gigya / SAP** (empresa multinacional) — 6 peticiones

Sistema de gestión de identidad y autenticación. Se carga para todos los usuarios, incluidos los no registrados.

#### f) **Chartbeat** (empresa estadounidense) — 2 peticiones (bloqueadas)

Analítica editorial en tiempo real.

#### g) **OneTrust** (empresa estadounidense) — 3 peticiones

Plataforma de gestión de consentimiento de cookies. Paradójicamente, el sistema diseñado para proteger la privacidad del usuario (gestionar consentimiento RGPD) también es bloqueado parcialmente.

### 3.3 Volumen de código ejecutado en el navegador del ciudadano

Para ver un vídeo de televisión pública, el navegador descarga y ejecuta:

| Componente | Tamaño |
|-----------|--------|
| Player de vídeo THEOplayer (pf_video.js) | **7.7 MB** |
| SDK de autenticación Gigya | 626 KB |
| Google IMA SDK (publicidad) | 466 KB |
| Conviva AppTracker (telemetría y vigilancia) | 423 KB |
| Otros scripts (jQuery, analytics, tracking) | ~800 KB |
| **Total aproximado de JavaScript** | **~10 MB** |

En comparación, la descarga completa del *contenido de vídeo* que el ciudadano desea ver requeriría aproximadamente 500 MB-1 GB. La infraestructura de tracking y publicidad supone un 1-2% del volumen del vídeo, pero es el 100% de lo que se ejecuta cuando el vídeo no se reproduce.

---

## 4. Análisis de las Implicaciones

### 4.1 Vulneración del derecho de acceso al servicio público

El ciudadano que utiliza herramientas de protección de privacidad en su navegador — herramientas legales, recomendadas por organismos de protección de datos y ampliamente utilizadas (se estima que un 30-40% de usuarios europeos emplean algún tipo de bloqueador) — queda excluido del acceso al contenido de televisión pública.

RTVE no muestra ningún mensaje que explique al usuario por qué el vídeo no se reproduce, ni ofrece alternativa alguna (como desactivar específicamente el bloqueador para su dominio, o reproducir sin publicidad).

### 4.2 Desproporción entre servicio público y tracking comercial

La siguiente tabla ilustra la desproporción entre las peticiones dedicadas al contenido y las dedicadas al tracking:

| Categoría | Peticiones | Porcentaje |
|-----------|-----------|------------|
| Contenido público (HTML, CSS, imágenes del programa) | ~50 | 32% |
| Funcionalidad del player | ~20 | 13% |
| **Tracking, publicidad y perfilado** | **~68** | **43%** |
| APIs de RTVE (metadatos, auth) | ~19 | 12% |

**El 43% del tráfico de red en una página de servicio público es tracking comercial.**

### 4.3 Conviva como intermediario de red total

La interceptación de `XMLHttpRequest`, `fetch` y `WebSocket` por Conviva es técnicamente equivalente a un **man-in-the-middle (intermediario)** a nivel de aplicación web. Todas las peticiones que la página realice — incluyendo datos de autenticación, preferencias del usuario, o comunicaciones con otros servicios — son potencialmente observables por el código de Conviva.

Esto no es una interpretación: es la consecuencia directa y verificable del código que se ejecuta en el navegador. Las funciones nativas del navegador para comunicaciones de red son **reemplazadas** por funciones de Conviva que procesan los datos y luego llaman a las originales.

### 4.4 Tracking sobre contenido no consumido

Los 24 paquetes de telemetría enviados a Conviva se producen **aunque el vídeo no se reproduzca**. El servicio registra la intención del ciudadano de ver contenido, el programa que quería ver, y datos del dispositivo, sin que el ciudadano haya conseguido acceder al contenido.

---

## 5. Normativa Potencialmente Afectada

### 5.1 Reglamento General de Protección de Datos (RGPD)

- **Principio de minimización (art. 5.1.c):** Los datos recogidos deben ser adecuados, pertinentes y limitados a lo necesario. 24 peticiones de telemetría sobre un vídeo que no se reproduce vulneran este principio.
- **Base jurídica del tratamiento (art. 6):** El tratamiento de datos mediante Conviva, Adobe Audience Manager y VWO requiere una base jurídica clara. Para un servicio público, el consentimiento no puede ser la base si el servicio queda condicionado a su aceptación.
- **Transferencias internacionales (art. 44-49):** Conviva (USA), Adobe/Demdex (USA), VWO (India), Google (USA) — todas implican transferencia internacional de datos personales.

### 5.2 Ley 17/2006 de la Radio y la Televisión de Titularidad Estatal

- **Art. 3:** RTVE debe garantizar la universalidad de su servicio y el acceso a sus contenidos.
- **Principios de servicio público:** La utilización de servicios de perfilado comercial como Adobe Audience Manager es difícilmente compatible con la función de servicio público.

### 5.3 Ley General de Comunicación Audiovisual (Ley 13/2022)

- **Art. 109-113:** Regulación de la publicidad en servicios públicos de comunicación audiovisual. La arquitectura técnica que condiciona la reproducción a la carga de publicidad podría constituir una forma de publicidad encubierta u obligatoria.

---

## 6. Solicitudes

En virtud de lo expuesto, se solicita al Defensor del Espectador:

1. **Que investigue** si RTVE tiene conocimiento de que la reproducción de vídeo en RTVE Play está condicionada a la carga de servicios publicitarios de terceros, y si existe una decisión deliberada de no implementar mecanismos de degradación graciosa.

2. **Que requiera explicaciones** sobre la necesidad y proporcionalidad de utilizar los siguientes servicios en una plataforma pública:
   - Conviva (con capacidad de grabación de sesión e interceptación total de red)
   - Adobe Audience Manager / Demdex (perfilado comercial)
   - VWO (A/B testing comercial)

3. **Que solicite** la implementación de un mecanismo de reproducción de vídeo que funcione independientemente de la carga de servicios de publicidad y tracking, garantizando el acceso universal al contenido público.

4. **Que traslade** este informe a la Agencia Española de Protección de Datos (AEPD) para que evalúe:
   - La proporcionalidad de la interceptación de comunicaciones de red por Conviva.
   - La capacidad de grabación de sesión incluida en el código.
   - Las transferencias internacionales de datos a USA e India.
   - El envío de 24 paquetes de telemetría sobre contenido no consumido.

5. **Que requiera a RTVE** la publicación de un informe de transparencia sobre los servicios de tracking utilizados en RTVE Play, incluyendo:
   - Listado completo de terceros con acceso a datos de los usuarios.
   - Volumen de datos transmitidos a cada tercero.
   - Finalidad específica de cada servicio.
   - Período de retención de datos.
   - Coste económico de cada servicio contratado.

---

## 7. Datos de la Evidencia

Toda la evidencia técnica detallada en este informe está disponible en formato verificable:

| Fichero | Contenido | Tamaño |
|---------|-----------|--------|
| `www.rtve.es.har` | Captura completa del tráfico de red | 18.2 MB |
| `TVE_files/convivaAppTracker-v1_4_3.js` | Código fuente de Conviva | 423 KB |
| `TVE_files/pf_video.js` (vía redirect) | Player de vídeo THEOplayer | 7.7 MB |
| `TVE_files/config_video.js` | Configuración de dependencias | — |
| `TVE_files/init_pfvideo.js` | Inicializador del player | 175 KB |
| `TVE_files/gtm.js` | Google Tag Manager (carga Conviva) | — |
| `01_informe_tecnico_departamento_IT.md` | Informe técnico complementario | — |

El fichero HAR es un formato estándar (HTTP Archive) que puede ser analizado por cualquier experto técnico independiente utilizando herramientas de código abierto.

---

## 8. Datos de Contacto

| Campo | Valor |
|-------|-------|
| **Remitente** | [Nombre del ciudadano / organización] |
| **DNI/NIF** | [Número de identificación] |
| **Correo electrónico** | [Email de contacto] |
| **Fecha del incidente** | [Fecha de la captura HAR] |
| **Navegador utilizado** | Google Chrome / Safari (WebInspector/537.36) |
| **Extensión de bloqueo** | [Nombre del ad blocker] |

---

*Nota: Este informe ha sido elaborado a partir de un análisis técnico del código JavaScript públicamente visible que RTVE sirve a sus usuarios, y de una captura de tráfico de red legítima del propio navegador del usuario. No se ha realizado ningún acceso no autorizado a sistemas de RTVE.*

---

*Fin del informe.*
