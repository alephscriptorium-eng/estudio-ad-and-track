# Caso RTVE Play: Bloqueo de Contenido Publico y Vigilancia Digital

RTVE Play bloquea la reproduccion de video a usuarios con ad blocker. Un analisis forense revela que para ver un solo video, la pagina contacta con **29 dominios** de 12+ empresas, incluyendo un servicio (Conviva) que intercepta todas las comunicaciones de red del navegador.

Este repositorio documenta los hallazgos tecnicos y contiene borradores de acciones legales a nivel nacional e internacional.

---

## Que se encontro

- **Bloqueo deliberado:** THEOplayer 9.4.1 detecta el ad blocker y bloquea el contenido. RTVE configuro un fallback a fichero vacio (`js/vacio`) que garantiza el bloqueo. El reproductor soporta reproducir sin anuncios, pero RTVE eligio bloquearlo.
- **29 dominios de terceros** contactados por pagina (Google, Adobe, Conviva, Chartbeat, VWO, etc.)
- **Interceptacion global de red** por Conviva: sustituye XMLHttpRequest, fetch y WebSocket del navegador por wrappers propios
- **Session replay** integrado en Conviva (grabacion de sesion completa del usuario via rrweb)
- **Transferencias a EEUU** sin garantias claras post-Schrems II (5+ proveedores estadounidenses)
- **Telemetria sin contenido:** 24 heartbeats de Conviva se envian aunque el video nunca se reproduzca

---

## Estructura del repositorio

```
INFORMES/                              Analisis tecnicos
  01_informe_tecnico_departamento_IT    Arquitectura completa: 157 peticiones, 29 dominios, Conviva, cadena de carga
  02_analisis_mecanismo_deteccion       Codigo desofuscado de THEOplayer: error 10001, fallback a fichero vacio
  03_informe_defensor_del_espectador    Informe orientado al Defensor del Espectador

ACCIONES/                              Borradores de acciones legales
  01_reclamacion_defensora_espectador   Defensora del Espectador de RTVE
  02_solicitud_portal_transparencia     Portal de Transparencia (Ley 19/2013): contratos, metricas, EIPD
  03_reclamacion_AEPD                   Agencia Espanola de Proteccion de Datos: Conviva, transfers, consent
  04_denuncia_CNMC                      CNMC: incompatibilidad con servicio publico, incentivo a Play+
  05_propuesta_pregunta_parlamentaria   Congreso/Senado: 5 preguntas escritas + 1 oral

ACCIONES/RGPD_DIGITAL/                 Elevacion internacional - Eje datos y regulacion
  01_noyb                               noyb (Viena): Schrems II, transferencias EEUU
  02_EDRi                               EDRi (Bruselas): amplificacion, red de 40+ organizaciones
  03_EDPB                               EDPB (Bruselas): mecanismo de cooperacion transfronteriza
  04_EFF                                EFF (San Francisco): derecho al ad blocking
  05_DG_CONNECT                         Comision Europea (Bruselas): AVMSD, DMA, soberania digital

ACCIONES/CONSUMO_DDHH/                 Elevacion internacional - Eje consumidores y derechos humanos
  01_BEUC                               Org. Europea de Consumidores: accion representativa via FACUA/OCU
  02_Access_Now                         Access Now (Bruselas/NY): vigilancia corporativa por institucion publica
  03_Privacy_International              Privacy International (Londres): data exploitation, Conviva
  04_EBU                                Union Europea de Radiodifusion (Ginebra): presion interna, RTVE es miembro
  05_UN_relator                         Relator Especial ONU Libertad de Expresion (Ginebra)
```

---

## Evidencia tecnica (no incluida en el repositorio)

La evidencia primaria no se publica en este repositorio porque contiene datos de sesion del navegador (cookies, tokens, identificadores). Se conserva localmente y se adjuntara a las reclamaciones:

- Captura HAR del trafico de red (18 MB, 157 peticiones)
- Logs de consola del navegador
- 44 ficheros JavaScript descargados de la pagina
- HTML completo de la pagina guardada

---

## Orden recomendado de presentacion

**Fase 1 - Nacional:**
1. Portal de Transparencia (las respuestas refuerzan todo lo demas)
2. AEPD (puede sancionar)
3. Defensora del Espectador (presion interna)
4. CNMC (servicio publico)
5. Preguntas parlamentarias

**Fase 2 - Internacional (simultaneo a Fase 1):**
6. noyb + EDPB (via AEPD)
7. BEUC + Access Now

**Fase 3 - Amplificacion:**
8. EDRi + EFF + Privacy International
9. EBU (presion interna desde la propia organizacion de RTVE)

**Fase 4 - Cuando haya masa critica:**
10. DG CONNECT (Comision Europea)
11. Relator Especial ONU

---

## Estado

Todos los documentos contienen campos `[COMPLETAR]` para datos personales. Ningun documento ha sido presentado aun.

---

## Contexto legal

- RTVE es la Corporacion de Radio y Television Espanola, financiada con Presupuestos Generales del Estado
- Ley 17/2006, de la radio y la television de titularidad estatal
- Ley 13/2022, General de Comunicacion Audiovisual (transpone la AVMSD)
- Reglamento (UE) 2016/679 (RGPD)
- Ley 19/2013, de transparencia, acceso a la informacion publica y buen gobierno
