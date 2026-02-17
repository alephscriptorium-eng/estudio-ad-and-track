# Reclamacion transfronteriza ante el European Data Protection Board (EDPB)

**Destinatario:** European Data Protection Board (EDPB)
**Sede:** Rue Wiertz 60, 1047 Bruselas, Belgica
**Contacto:** EDPB@edpb.europa.eu / edpb.europa.eu
**Mecanismo:** Art. 60-65 RGPD (cooperacion y coherencia)
**Idioma:** Ingles
**Coste:** Gratuito

**Nota procedimental:** El EDPB no acepta reclamaciones directas de ciudadanos. La via procesal correcta es presentar la reclamacion ante la autoridad de proteccion de datos nacional (AEPD en Espana) e invocar el mecanismo de cooperacion transfronteriza del art. 60 RGPD. Este documento esta disenado para acompanar la reclamacion ante la AEPD y solicitar que active el mecanismo de ventanilla unica (one-stop-shop) dado el caracter transfronterizo del tratamiento.

---

## Solicitud de activacion del mecanismo de cooperacion transfronteriza

### Dirigido a: Agencia Espanola de Proteccion de Datos (AEPD)
### Con copia informativa a: EDPB Secretariat

---

### 1. Objeto

Solicito que la AEPD, al tramitar la reclamacion interpuesta contra la Corporacion RTVE por el tratamiento de datos en la plataforma RTVE Play, active el mecanismo de cooperacion transfronteriza previsto en los articulos 60 a 65 del RGPD, dada la participacion de multiples encargados de tratamiento con establecimiento principal fuera de Espana.

### 2. Justificacion del caracter transfronterizo

El tratamiento de datos realizado por RTVE Play involucra a los siguientes encargados/corresponsables con establecimiento en otros Estados miembros o terceros paises:

**Google LLC (IMA SDK, Tag Manager)**

- Establecimiento UE: Google Ireland Ltd., Irlanda
- Sede global: Mountain View, CA, USA
- Funcion: Publicidad, gestion de tags
- Autoridad competente potencial: DPC Ireland

**Adobe Inc. (Audience Manager, VisitorAPI)**

- Establecimiento UE: Adobe Systems Software Ireland Ltd., Irlanda
- Sede global: San Jose, CA, USA
- Funcion: Perfilado de audiencia, identificacion de visitantes
- Autoridad competente potencial: DPC Ireland

**Conviva Inc. (AppTracker)**

- Establecimiento UE: Sin establecimiento UE conocido
- Sede global: San Jose, CA, USA
- Funcion: Telemetria, interceptacion de red, session replay
- Autoridad competente potencial: AEPD (como autoridad del responsable)

**Chartbeat Inc.**

- Establecimiento UE: Sin establecimiento UE conocido
- Sede global: Nueva York, USA
- Funcion: Analitica en tiempo real
- Autoridad competente potencial: AEPD

**Wingify (VWO)**

- Establecimiento UE: Sin establecimiento UE conocido
- Sede global: Delhi, India
- Funcion: A/B testing, comportamiento de usuario
- Autoridad competente potencial: AEPD

**OneTrust LLC**

- Establecimiento UE: OneTrust Technology Ltd., Irlanda/UK
- Sede global: Atlanta, GA, USA
- Funcion: Gestion de consentimiento
- Autoridad competente potencial: DPC Ireland

**THEO Technologies (THEOplayer)**

- Establecimiento UE: THEO Technologies NV, Belgica
- Sede global: Lovaina, Belgica
- Funcion: Reproductor de video con deteccion de ad blocker
- Autoridad competente potencial: APD Belgium

### 3. Tratamientos con dimension transfronteriza

#### 3.1 Google LLC / Google Ireland Ltd.

Google actua como encargado del tratamiento para la publicidad (IMA SDK) y la gestion de etiquetas (Tag Manager). El ad tag configurado por RTVE envia a servidores de Google: dimensiones de pantalla del usuario, user agent, consent string (estado del consentimiento), estado de suscripcion (parametro PLAY_PLUS), e identificadores de contenido.

Google Ireland Ltd. es el establecimiento principal en la UE, lo que hace competente a la DPC Ireland como autoridad principal para el tratamiento realizado por Google.

#### 3.2 Adobe Inc. / Adobe Systems Software Ireland Ltd.

Adobe Audience Manager y VisitorAPI se cargan en cada sesion de RTVE Play, generando: un identificador unico de visitante (Marketing Cloud ID / MID), datos de perfilado de audiencia, y segmentacion de usuarios.

Adobe Systems Software Ireland Ltd. es el establecimiento UE, haciendo competente a la DPC Ireland.

#### 3.3 Conviva Inc.

Conviva realiza el tratamiento mas intrusivo:

- Interceptacion global de XMLHttpRequest, fetch y WebSocket
- Capacidad de session replay (rrweb)
- 24 heartbeats de telemetria incluso sin reproduccion de video
- Transferencia a appgw.conviva.com (EE.UU.)

Sin establecimiento conocido en la UE, la competencia principal recae en la AEPD como autoridad del responsable del tratamiento (RTVE).

#### 3.4 THEO Technologies NV (Belgica)

THEOplayer es el reproductor de video que incluye la funcionalidad de deteccion de ad blocker. THEO Technologies tiene su sede en Lovaina, Belgica, lo que puede involucrar a la APD belga si THEOplayer actua como encargado del tratamiento para la deteccion y clasificacion de usuarios.

### 4. Base juridica para la cooperacion transfronteriza

- Articulo 56(1) RGPD: La autoridad de control del establecimiento principal del responsable o encargado sera competente como autoridad de control principal.
- Articulo 60 RGPD: La autoridad de control principal cooperara con las demas autoridades de control interesadas.
- Articulo 61 RGPD: Asistencia mutua entre autoridades.
- Articulo 65 RGPD: Resolucion de litigios por el EDPB en caso de desacuerdo.

### 5. Solicitud

Solicito a la AEPD que:

1. Identifique a todos los encargados y corresponsables del tratamiento involucrados en RTVE Play y sus establecimientos principales en la UE.
2. Active el mecanismo de cooperacion del art. 60 RGPD respecto a los tratamientos realizados por Google Ireland y Adobe Ireland en el contexto de RTVE Play.
3. Comunique al EDPB la existencia de este caso transfronterizo.
4. Coordine con la DPC Ireland y la APD belga la investigacion de los respectivos encargados de tratamiento.
5. Solicite informacion a Conviva Inc. sobre su representante en la UE (art. 27 RGPD) y, en su defecto, informe al EDPB de la ausencia de representante.

### 6. Evidencia

Se adjunta el mismo paquete de evidencia tecnica que la reclamacion principal ante la AEPD:

1. Informe tecnico completo con analisis de la cadena de carga
2. Analisis del mecanismo de deteccion de ad blocker con codigo fuente desofuscado
3. Captura HAR del trafico de red completo (157 peticiones a 29 dominios)
4. Logs de consola del navegador
5. Codigo fuente descargado (44 ficheros JavaScript)

---

[Nombre y apellidos]
[DNI]
[Fecha]
[Firma]

---

**Nota practica:** Este documento se presenta como anexo a la reclamacion ante la AEPD (documento 03_reclamacion_AEPD.md). Se recomienda enviar copia informativa al EDPB Secretariat por correo electronico a EDPB@edpb.europa.eu indicando que se ha solicitado la activacion del mecanismo de cooperacion transfronteriza ante la AEPD.
