# Reclamacion a la Defensora del Espectador de RTVE

**Via de presentacion:** Formulario web en rtve.es > "Contacta" > "Defensora del Espectador", o por correo electronico a defensora.espectador@rtve.es
**Destinatario:** Defensora del Espectador, Corporacion RTVE
**Asunto:** Bloqueo de reproduccion de video en RTVE Play a usuarios con bloqueador de publicidad

---

Estimada Defensora del Espectador:

Me dirijo a usted en ejercicio de mi derecho como ciudadano y contribuyente para trasladar una queja fundamentada sobre el funcionamiento del servicio RTVE Play.

## Hechos

1. Al acceder a contenido de video en RTVE Play (por ejemplo, programas de La 1 como "La Revuelta") con un navegador que tiene instalada una extension de bloqueo de publicidad, la reproduccion del video **se bloquea completamente**. La navegacion por la web funciona con normalidad; unicamente se impide el acceso al contenido audiovisual.

2. He realizado un analisis tecnico del funcionamiento de la pagina que revela lo siguiente:

   a) RTVE utiliza el reproductor comercial **THEOplayer 9.4.1**, que incluye una funcion integrada de deteccion de bloqueadores de publicidad.

   b) Cuando el reproductor detecta que el **Google IMA SDK** (sistema de publicidad de Google) no esta disponible, **clasifica cualquier fallo como "ADBLOCK"** y bloquea la reproduccion del contenido, mostrando el mensaje "Turn off your AdBlocker and try again" (en ingles, ni siquiera localizado al castellano).

   c) RTVE ha configurado deliberadamente un **fichero vacio de respaldo** (`js2.rtve.es/js/vacio`) que garantiza que, cuando la publicidad no se carga, el video nunca se reproduce. THEOplayer ofrece la opcion de continuar la reproduccion sin anuncios cuando estos fallan, pero RTVE ha elegido explicitamente la opcion de bloquear el contenido.

   d) Para reproducir un solo video, la pagina contacta con **29 dominios diferentes**, incluyendo multiples servicios de tracking y perfilado de terceros (Google, Adobe, Conviva, Chartbeat, VWO, entre otros).

## Fundamento

RTVE es la Corporacion de Radio y Television Espanola, un servicio publico esencial financiado mayoritariamente con los Presupuestos Generales del Estado. Su mision de servicio publico, recogida en la Ley 17/2006 de la radio y la television de titularidad estatal, incluye garantizar el acceso universal a su contenido.

Condicionar el acceso a contenido publico a la aceptacion de publicidad servida por plataformas privadas extranjeras (Google, con servidores en Estados Unidos) contradice esta mision de servicio publico. Los ciudadanos que instalan bloqueadores de publicidad lo hacen, en muchos casos, por razones legitimas de seguridad, privacidad y rendimiento de sus dispositivos.

## Solicitud

1. Que se modifique la configuracion del reproductor de video para permitir la **reproduccion de contenido sin publicidad** cuando el sistema publicitario no este disponible, como alternativa al bloqueo total.

2. Que se reduzca la dependencia de servicios de tracking de terceros (actualmente 29 dominios) para acceder a un servicio publico.

3. Que se traduzcan al castellano todos los mensajes de error del reproductor, ya que actualmente aparecen en ingles.

4. Que se informe publicamente sobre la politica de RTVE respecto al bloqueo de contenido por uso de bloqueadores de publicidad y su compatibilidad con la mision de servicio publico.

## Evidencia adjunta

Adjunto a esta reclamacion los siguientes documentos tecnicos:
- Informe tecnico con analisis completo de la cadena de carga del reproductor
- Analisis del mecanismo de deteccion de ad blocker con codigo fuente desofuscado
- Captura HAR del trafico de red completo de la pagina
- Logs de consola del navegador

Quedo a su disposicion para cualquier aclaracion.

Atentamente,

[Nombre y apellidos]
[DNI]
[Correo electronico]
[Fecha]
