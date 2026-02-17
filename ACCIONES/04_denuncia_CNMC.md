# Denuncia ante la Comision Nacional de los Mercados y la Competencia (CNMC)

**Via de presentacion:** Sede electronica de la CNMC (sede.cnmc.gob.es) > "Denuncias y reclamaciones" > "Presentar denuncia en materia de competencia" / Direccion General de Telecomunicaciones y Sector Audiovisual
**Base legal:** Ley 7/2010, General de la Comunicacion Audiovisual; Ley 13/2022, General de Comunicacion Audiovisual (vigente); Ley 17/2006, de la radio y la television de titularidad estatal
**Denunciado:** Corporacion de Radio y Television Espanola (RTVE), CIF Q-2876002-C

---

## 1. Datos del denunciante

- Nombre y apellidos: [COMPLETAR]
- DNI/NIE: [COMPLETAR]
- Domicilio: [COMPLETAR]
- Correo electronico: [COMPLETAR]

## 2. Entidad denunciada

- Denominacion: Corporacion de Radio y Television Espanola (Corporacion RTVE)
- CIF: Q-2876002-C
- Domicilio: Avenida de Radio Television, 4, 28223 Pozuelo de Alarcon, Madrid

## 3. Hechos denunciados

### 3.1 Bloqueo de acceso a contenido del servicio publico audiovisual

RTVE Play, la plataforma de video bajo demanda de la Corporacion RTVE, **bloquea la reproduccion de contenido audiovisual** a los usuarios que disponen de extensiones de bloqueo de publicidad en su navegador.

El mecanismo tecnico, documentado mediante analisis forense del codigo fuente, funciona de la siguiente forma:

1. RTVE utiliza el reproductor comercial THEOplayer 9.4.1 con la integracion publicitaria Google IMA SDK.
2. Antes de reproducir el contenido, el reproductor intenta cargar el sistema publicitario de Google.
3. Si el sistema publicitario no esta disponible (por ejemplo, porque el usuario tiene un bloqueador de publicidad), el reproductor **bloquea la reproduccion** del contenido en lugar de reproducirlo sin anuncios.
4. RTVE ha configurado deliberadamente un fichero de respaldo vacio (`js2.rtve.es/js/vacio`) que garantiza este comportamiento de bloqueo.

El reproductor THEOplayer ofrece la opcion de reproducir el contenido sin anuncios cuando estos no estan disponibles, pero **RTVE ha elegido explicitamente la opcion de bloquear** el acceso al contenido.

### 3.2 Incentivo encubierto a la suscripcion de pago

El analisis tecnico revela que el ad tag publicitario incluye un parametro `PLAY_PLUS` que toma el valor "0" (usuario gratuito) o "1" (suscriptor de pago). Los suscriptores de pago de RTVE Play+ reciben un tratamiento publicitario diferente (potencialmente menos anuncios o sin anuncios).

La combinacion de:
- Bloqueo de contenido si el usuario tiene ad blocker
- Menos publicidad si el usuario paga la suscripcion Play+

crea un **incentivo de pago encubierto** para acceder a contenido que, como servicio publico, deberia estar disponible de forma gratuita y universal.

### 3.3 Dependencia de infraestructura publicitaria privada extranjera

La reproduccion de contenido del servicio publico espanol de television esta tecnicamente condicionada a la disponibilidad de servidores de Google (imasdk.googleapis.com) ubicados fuera de Espana. Si estos servidores no estan disponibles por cualquier motivo, el contenido publico no se puede reproducir.

## 4. Fundamento juridico

### 4.1 Ley 13/2022, General de Comunicacion Audiovisual

- **Articulo 40 (Mision de servicio publico):** El servicio publico de comunicacion audiovisual tiene como mision difundir contenidos que fomenten los valores constitucionales, la formacion de una opinion publica plural y la participacion democratica. El bloqueo de contenido por motivos publicitarios contradice esta mision.

- **Articulo 43 (Obligaciones del servicio publico):** RTVE debe garantizar el acceso universal a su oferta de programacion.

### 4.2 Ley 17/2006, de la radio y la television de titularidad estatal

- **Articulo 2 (Servicio publico):** La funcion de servicio publico comprende la produccion de contenidos y la edicion y difusion de canales de television. El acceso a estos contenidos no debe estar condicionado a la aceptacion de publicidad comercial de terceros.

- **Articulo 3 (Principios de la produccion):** RTVE debe garantizar la universalidad de sus contenidos.

### 4.3 Mandato-marco a RTVE

El Mandato-marco de RTVE establece que la Corporacion debe ofrecer sus contenidos a traves de su plataforma digital de forma abierta y accesible para todos los ciudadanos.

## 5. Evidencia

Se adjunta:
1. Informe tecnico con analisis de la cadena de carga del reproductor (157 peticiones de red, 29 dominios)
2. Analisis del mecanismo de deteccion con codigo fuente desofuscado que demuestra la configuracion deliberada del bloqueo
3. Captura HAR del trafico de red completo
4. Codigo fuente descargado de la pagina

## 6. Solicitud

Solicito a la CNMC que:

1. Investigue si el bloqueo de contenido audiovisual del servicio publico por motivos publicitarios es compatible con las obligaciones de servicio publico de RTVE.

2. Determine si la configuracion de bloqueo deliberado (fichero vacio de respaldo, pre-roll obligatorio) constituye una practica contraria a los principios de acceso universal al servicio publico audiovisual.

3. Examine si el esquema de incentivo a la suscripcion de pago (Play+) mediante bloqueo de contenido a usuarios con ad blocker es compatible con la mision de servicio publico.

4. Adopte las medidas que estime oportunas para garantizar el acceso universal al contenido de RTVE Play.

---

[Nombre y apellidos]
[DNI]
[Fecha]
[Firma]

---

**Nota:** La denuncia ante la CNMC es gratuita y puede presentarse telematicamente a traves de la sede electronica con certificado digital o Cl@ve.
