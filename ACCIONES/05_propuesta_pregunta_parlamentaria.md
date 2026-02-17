# Propuesta de Pregunta Parlamentaria sobre RTVE Play

**Via:** Dirigirse a un grupo parlamentario del Congreso de los Diputados o del Senado para que formule pregunta escrita al Gobierno o a la Corporacion RTVE a traves de la Comision Mixta de Control Parlamentario de la Corporacion RTVE y sus Sociedades.
**Base:** Articulo 185 del Reglamento del Congreso (preguntas escritas); articulo 20.1.d) CE (derecho a la informacion)

---

## Contexto para el grupo parlamentario

RTVE Play, la plataforma de video bajo demanda de la television publica espanola, bloquea la reproduccion de contenido a los ciudadanos que utilizan bloqueadores de publicidad en sus navegadores.

Un analisis tecnico forense de la pagina revela que:

- RTVE ha configurado **deliberadamente** el reproductor de video (THEOplayer, licencia comercial) para que **impida la reproduccion** del contenido si la publicidad de Google (IMA SDK) no se carga.
- El reproductor ofrece la opcion de reproducir el contenido sin anuncios, pero RTVE ha elegido la opcion de bloquear.
- Para ver un solo video, la pagina del ciudadano contacta con **29 dominios diferentes**, muchos de ellos servicios de perfilado y tracking de empresas estadounidenses (Google, Adobe, Conviva).
- El sistema de telemetria integrado (Conviva) **intercepta todas las comunicaciones de red del navegador** e incluye capacidad de grabacion de sesion completa del usuario.
- Los suscriptores de pago (Play+) reciben un tratamiento publicitario diferente, creando un incentivo economico para acceder a contenido publico.

---

## Propuesta de preguntas escritas

### Pregunta 1: Sobre el bloqueo de contenido

> ¿Conoce el Gobierno que la Corporacion RTVE bloquea la reproduccion de contenido audiovisual en su plataforma RTVE Play a los ciudadanos que utilizan bloqueadores de publicidad en sus navegadores? ¿Considera el Gobierno que esta practica es compatible con la mision de servicio publico de RTVE y con el principio de acceso universal a los contenidos de la television publica recogido en la Ley 17/2006 y la Ley 13/2022?

### Pregunta 2: Sobre la dependencia tecnologica

> ¿Puede el Gobierno informar sobre el contrato vigente entre RTVE y THEO Technologies para la licencia del reproductor de video THEOplayer, incluyendo su importe y condiciones? ¿Puede igualmente informar sobre las condiciones del acuerdo con Google para la integracion del sistema publicitario Google IMA SDK, del que depende tecnicamente la reproduccion de contenido publico?

### Pregunta 3: Sobre la proteccion de datos

> ¿Ha realizado RTVE una Evaluacion de Impacto en la Proteccion de Datos (EIPD) para el servicio RTVE Play, teniendo en cuenta que la pagina de reproduccion de video contacta con 29 dominios diferentes, que el servicio de telemetria Conviva intercepta todas las comunicaciones de red del navegador del ciudadano y que se realizan transferencias internacionales de datos a al menos cinco proveedores estadounidenses?

### Pregunta 4: Sobre ingresos y modelo de negocio

> ¿Cuales han sido los ingresos publicitarios generados por RTVE Play durante los ejercicios 2023, 2024 y 2025? ¿Que porcentaje de usuarios de RTVE Play son detectados como usuarios de bloqueadores de publicidad segun los sistemas de telemetria de la plataforma? ¿Cuantas sesiones de video se han bloqueado por este motivo?

### Pregunta 5: Sobre el incentivo a la suscripcion de pago

> ¿Puede el Gobierno confirmar que los suscriptores de pago de RTVE Play+ reciben un tratamiento publicitario diferente (menos anuncios o sin anuncios) respecto a los usuarios gratuitos? En caso afirmativo, ¿considera el Gobierno que la combinacion de bloqueo de contenido a usuarios con ad blocker e incentivo a la suscripcion de pago es compatible con la naturaleza de servicio publico de RTVE?

---

## Propuesta de pregunta oral en Comision Mixta de Control de RTVE

> Senor/a Presidente/a de la Corporacion RTVE: ¿Puede explicar ante esta Comision por que RTVE Play bloquea la reproduccion de contenido publico, financiado con los Presupuestos Generales del Estado, a los ciudadanos que utilizan bloqueadores de publicidad en sus navegadores, habiendo configurado deliberadamente el reproductor de video para impedir el acceso al contenido cuando la publicidad de Google no se carga? ¿Es consciente de que el reproductor utilizado ofrece la opcion de reproducir el contenido sin anuncios y que RTVE ha elegido explicitamente la opcion de bloquear? ¿Puede informar sobre cuantos ciudadanos han visto bloqueado su acceso al contenido publico por este motivo?

---

## Documentacion de apoyo

Se facilita al grupo parlamentario:

1. Informe tecnico completo con analisis de la arquitectura de RTVE Play
2. Analisis del mecanismo de deteccion de ad blocker con codigo fuente desofuscado
3. Captura de trafico de red (fichero HAR) como prueba pericial
4. Logs del navegador

---

## Como contactar con un grupo parlamentario

1. **Congreso de los Diputados:** Acceder a la web del Congreso (congreso.es) > Diputados > Buscar diputados de la Comision Mixta de Control Parlamentario de RTVE > Contactar via email o formulario del grupo parlamentario.

2. **Senado:** Acceder a senado.es > Composicion > Senadores > Contactar con senadores miembros de la Comision Mixta.

3. **Alternativamente:** Contactar con el portavoz o responsable de politica digital/telecomunicaciones de cualquier grupo parlamentario.

Las preguntas escritas al Gobierno deben ser respondidas en un plazo de 30 dias (articulo 190 del Reglamento del Congreso).
