# Reclamacion ante la Agencia Espanola de Proteccion de Datos (AEPD)

**Via de presentacion:** Sede electronica de la AEPD (sedeagpd.gob.es) > "Reclamaciones" > "Presentar una reclamacion"
**Base legal:** Reglamento (UE) 2016/679 (RGPD) y Ley Organica 3/2018 (LOPDGDD)
**Responsable del tratamiento reclamado:** Corporacion de Radio y Television Espanola (RTVE), CIF Q-2876002-C

---

## 1. Datos del reclamante

- Nombre y apellidos: [COMPLETAR]
- DNI/NIE: [COMPLETAR]
- Domicilio: [COMPLETAR]
- Correo electronico: [COMPLETAR]
- Telefono: [COMPLETAR]

## 2. Datos del reclamado

- Denominacion: Corporacion de Radio y Television Espanola (Corporacion RTVE)
- CIF: Q-2876002-C
- Domicilio: Avenida de Radio Television, 4, 28223 Pozuelo de Alarcon, Madrid
- Web: www.rtve.es
- DPD (Delegado de Proteccion de Datos): dpd@rtve.es

## 3. Hechos reclamados

### 3.1 Descripcion general

Al acceder al servicio RTVE Play para visualizar contenido de video, la pagina web de RTVE realiza un tratamiento masivo de datos personales a traves de multiples servicios de terceros, sin que el usuario sea adecuadamente informado ni pueda ejercer un control efectivo sobre dicho tratamiento. He realizado un analisis tecnico forense de la pagina de reproduccion de video que documenta los siguientes hechos:

### 3.2 Intercepcion global de comunicaciones de red por Conviva

El servicio de telemetria **Conviva AppTracker v1.4.3** (proveedor: Conviva Inc., San Jose, California, EE.UU.), cargado a traves de Google Tag Manager en RTVE Play, realiza una **interceptacion sistematica de todas las comunicaciones de red del navegador** mediante la tecnica de "monkey-patching":

- Sustituye `XMLHttpRequest.prototype.open`, `.send` y `.setRequestHeader` por funciones propias
- Sustituye `window.fetch` por un interceptor propio
- Sustituye el constructor `WebSocket` y `WebSocket.prototype.send` por implementaciones propias

Esto significa que **toda comunicacion de red** que realice el navegador del usuario mientras la pagina de RTVE Play esta abierta (incluyendo peticiones a otros dominios, tabs o servicios) es potencialmente interceptada y procesada por Conviva, con transferencia de datos a servidores en Estados Unidos (`appgw.conviva.com`).

### 3.3 Capacidad de grabacion de sesion completa (session replay)

El codigo de Conviva AppTracker incluye un **modulo completo de grabacion de sesion de usuario** basado en la libreria `rrweb`, que captura:

- Movimientos del raton
- Clics del usuario
- Desplazamiento (scroll)
- Cambios en campos de formulario (inputs)
- Mutaciones del DOM (cambios en el contenido de la pagina)

Estas grabaciones se almacenan en IndexedDB local (`conviva-session-record-db`), se comprimen con gzip y se suben a Google Cloud Storage. La activacion depende de una configuracion remota descargada desde `rc.conviva.com`, lo que significa que RTVE o Conviva pueden activar la grabacion de sesiones sin conocimiento del usuario y sin necesidad de actualizar el codigo de la pagina.

### 3.4 Transferencias internacionales de datos sin base juridica clara

La pagina contacta con **29 dominios diferentes** para reproducir un solo video. Los siguientes implican transferencia internacional de datos a Estados Unidos:

| Servicio | Proveedor | Pais | Datos transferidos |
|----------|-----------|------|--------------------|
| Google IMA SDK | Google LLC | EE.UU. | Dimensiones pantalla, user agent, consent string, estado suscripcion |
| Adobe Audience Manager | Adobe Inc. | EE.UU. | Identificador unico de visitante (MID), perfilado de audiencia |
| Conviva | Conviva Inc. | EE.UU. | Telemetria completa de sesion, potencial session replay |
| Chartbeat | Chartbeat Inc. | EE.UU. | Comportamiento de navegacion en tiempo real |
| VWO | Wingify | India | Datos de A/B testing, comportamiento del usuario |

### 3.5 Numero excesivo de encargados de tratamiento sin informacion al usuario

La politica de privacidad de RTVE no detalla de forma suficiente la participacion de todos estos encargados de tratamiento ni la finalidad especifica de cada uno. El usuario medio no puede conocer que la visualizacion de un video implica compartir datos con al menos 12 empresas diferentes.

### 3.6 Telemetria sin reproduccion de contenido

He documentado que los 24 heartbeats de Conviva (POST a `conviva.com/ctp`) se envian **aunque el video nunca llegue a reproducirse** (porque el reproductor bloquea la reproduccion al detectar un ad blocker). Esto significa que RTVE recoge y transmite datos de telemetria sobre el usuario sin que este haya consumido ningun contenido.

### 3.7 Sistema de consentimiento deficiente

El banner de consentimiento de cookies esta gestionado por OneTrust, pero:
- El propio SDK de OneTrust (`cdn.cookielaw.org/scripttemplates/.../otBannerSdk.js`) es bloqueado por extensiones de privacidad comunes, con lo cual el mecanismo de consentimiento no se presenta al usuario.
- Multiples servicios de tracking (Conviva, Adobe, VWO) se cargan **antes** o **independientemente** de que el usuario haya prestado su consentimiento a traves de OneTrust.

## 4. Derechos presuntamente vulnerados

### 4.1 Principio de minimizacion de datos (art. 5.1.c RGPD)

La interceptacion global de XHR/fetch/WebSocket por Conviva excede manifiestamente lo necesario para la finalidad declarada (analitica de video). Monitorizar toda la comunicacion de red de la pagina no es proporcionado para medir la calidad de reproduccion de un video.

### 4.2 Principio de transparencia (art. 5.1.a RGPD) y deber de informacion (arts. 13-14 RGPD)

El usuario no es informado de forma clara y accesible sobre:
- La interceptacion global de comunicaciones de red
- La capacidad de grabacion de sesion
- El numero real de encargados de tratamiento involucrados
- Las transferencias internacionales a EE.UU.

### 4.3 Base juridica del tratamiento (art. 6 RGPD)

La telemetria que se realiza antes de que el usuario preste consentimiento y que continua aunque el usuario no consuma contenido carece de base juridica clara. El interes legitimo no puede amparar la interceptacion global de comunicaciones de red.

### 4.4 Transferencias internacionales (arts. 44-49 RGPD)

Tras la invalidacion del Privacy Shield (Schrems II, STJUE C-311/18), las transferencias de datos a EE.UU. requieren garantias adicionales. No consta que RTVE haya evaluado el riesgo de estas transferencias para cada uno de los 5+ proveedores estadounidenses involucrados.

### 4.5 Evaluacion de impacto (art. 35 RGPD)

El tratamiento a gran escala de datos de comportamiento de los usuarios de un servicio publico, con interceptacion de comunicaciones y capacidad de grabacion de sesion, requiere una Evaluacion de Impacto en la Proteccion de Datos (EIPD). No hay evidencia publica de que RTVE haya realizado dicha evaluacion para el servicio RTVE Play.

## 5. Evidencia aportada

Se adjunta la siguiente documentacion tecnica:

1. **Informe tecnico general** (`01_informe_tecnico_departamento_IT.md`): Analisis completo de la cadena de carga, inventario de servicios de terceros, analisis de Conviva AppTracker.

2. **Analisis de deteccion de ad blocker** (`02_analisis_mecanismo_deteccion_adblock.md`): Codigo desofuscado del mecanismo de deteccion, demostracion del fallback deliberado a fichero vacio.

3. **Captura HAR** (`www.rtve.es.har`): Trafico de red completo de la pagina (157 peticiones a 29 dominios).

4. **Logs de consola** (`logs.md`): Errores del navegador que demuestran las peticiones bloqueadas.

5. **Codigo fuente** (`TVE_files/`): 44 ficheros JavaScript descargados de la pagina como evidencia primaria verificable.

## 6. Solicitud

Solicito a la AEPD que:

1. Investigue si RTVE cumple con los principios de minimizacion y transparencia en el tratamiento de datos de los usuarios de RTVE Play.

2. Verifique la existencia y adecuacion de la Evaluacion de Impacto en la Proteccion de Datos para el servicio RTVE Play.

3. Examine la licitud de la interceptacion global de comunicaciones de red por Conviva AppTracker.

4. Compruebe si la funcionalidad de session replay esta activa en produccion y, en su caso, bajo que base juridica.

5. Verifique la adecuacion de las garantias para las transferencias internacionales de datos a proveedores estadounidenses.

6. Adopte las medidas correctoras que estime oportunas.

---

[Nombre y apellidos]
[DNI]
[Fecha]
[Firma]

---

**Nota:** La reclamacion ante la AEPD es gratuita y no requiere abogado ni procurador. Se puede presentar telematicamente con certificado digital o Cl@ve, o presencialmente en cualquier registro publico.
