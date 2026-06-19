
# Integración TAK

La aplicación Meshtastic admite la integración de Team Awareness Kit (TAK), lo que permite la interoperabilidad con ATAK (Android Team Awareness Kit), iTAK y otros sistemas compatibles con CoT (Cursor-on-Target) a través de la radio de malla LoRa, sin necesidad de celular o Internet.

## ¿Qué es TAK?

TAK es una plataforma de conciencia situacional ampliamente utilizada en contextos tácticos, de gestión de emergencias y de recreación al aire libre. Muestra las posiciones y el estado de los miembros del equipo en un mapa compartido. Meshtastic une a los usuarios de TAK con LoRa, por lo que los equipos se mantienen conectados sin requerir cobertura celular o de Internet.

## Roles de dispositivos admitidos

La integración de TAK funciona con dos roles de dispositivo:

| Icono | Papel | Descripción |
|------|------|-------------|
| ![TAK](../assets/screenshots/roleTak.png) | TAK | Rol TAK completo: envía informes de posición de CoT y puede transmitir paquetes de datos TAK. |
| ![TAK Tracker](../assets/screenshots/roleTakTracker.png) | Rastreador TAK | Función TAK de solo posición ligera. Menor consumo de energía, sin relé de paquetes. |

Establezca el rol del dispositivo en **Configuración → Dispositivo**.

> **Tip — Versión del firmware**
> El formato completo de cable TAK V2 (formas, rutas, marcadores, casevac, emergencia) requiere firmware **2.8.0 o posterior** en la radio conectada. El firmware más antiguo todavía es compatible con PLI y GeoChat sobre el formato V1 heredado; la aplicación vuelve a caer automáticamente.

## Pantalla del servidor TAK

**Configuración → Servidor TAK** es el único destino para todo lo relacionado con TAK. La pantalla está organizada de arriba a abajo para que pueda configurar su identidad, iniciar el servidor y emparejar un cliente ATAK / iTAK en una sola pasada.

### Identidad TAK

La primera sección, **TAK Identity**, controla el equipo a nivel de firmware y la identidad del rol que la radio adjunta a cada informe de posición:

| Escenario | Descripción |
|---------|-------------|
| Equipo | El color del equipo que se muestra a los clientes de TAK. El valor predeterminado es Cian; todos los colores estándar del equipo ATAK están disponibles. |
| Papel | Tu papel de TAK. Las opciones son Miembro del Equipo (por defecto), Líder de Equipo, HQ, Francotirador, Médico, Observador Avanzado, RTO y K9. |

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/takIdentitySection_dark.png">
  <img src="../assets/screenshots/takIdentitySection.png" alt="TAK Identity section with Team and Role pickers">
</picture>

Un botón **Guardar identidad TAK** aparece en la sección solo cuando hay cambios no guardados. Guardar envía un mensaje de administrador al nodo conectado; verá el cambio reflejado en los clientes TAK en el siguiente informe de posición.

> **Tip — ¿Los selectores de identidad están deshabilitados?**
> Los selectores permaneceron desactivados hasta que la radio conectada informe de la configuración de su módulo TAK a la aplicación. Esto suele ocurrir a los pocos segundos de conectarse: dale un momento, o desconéctate y vuelve a conectar si no aparece.

### Estado del servidor, habilitar y canal

Debajo de la sección de identidad:

- Un **indicador de estado** que muestra si el servidor TAK en la aplicación se está ejecutando y si su canal principal es adecuado para el uso de TAK.
- Un interruptor **Habilitar servidor TAK**.
- Un **selector de canales** para el canal LoRa, el servidor puentea entre los clientes TAK y la malla.
- **Modo de solo lectura** (trate la aplicación como un observador TAK que no reenvía CoT a la malla) y **relé de malla a CoT** alterna.

> **Tip — Requisitos del canal principal**
> El servidor TAK se ejecuta en **modo de solo lectura** hasta que su canal principal tenga un nombre no predeterminado y una clave de cifrado de 256 bits no predeterminada. Utilice el botón **Corregir canal** en el banner de advertencia para aplicar un preajuste TAK recomendado (Short Fast, nueva clave AES, nombre "TAK") con un solo toque.

### Certificados

Importe un paquete P12 (PKCS#12) o PEM para conexiones ATAK / iTAK protegidas por mTLS. La aplicación almacena los certificados cifrados en el llavero; no son visibles para otras aplicaciones ni para compartir archivos de iTunes/Finder.

### Paquete de datos

Exporte un paquete de datos TAK zip que puede cargar lateralmente en ATAK / iTAK. El cliente lo utiliza para encontrar y confiar en el servidor local de la aplicación sin introducir manualmente un host, puerto o certificado.

## Rutas de recepción

Cuando otro nodo en la malla envía una ruta CoT (`b-m-r`), La aplicación lo escribe automáticamente como un paquete de datos KML para`Documents/TAK Routes/`Y publica una notificación de iOS para que no te la pierdas:

| Campo | Contenido |
|-------|---------|
| Título | Ruta recibida |
| Subtítulo | _Signo de llamada de ruta_ (o "Ruta desconocida") |
| Cuerpo | Guardado en Archivos → Meshtastic → Rutas TAK. Abrir en iTAK para importar. |

iTAK ignora silenciosamente la ruta CoT recibida a través de su conexión de transmisión TCP, por lo que este respaldo le permite importar la ruta manualmente. Toque la notificación y, a continuación, en Archivos, vaya a **En mi iPhone → Meshtastic → Rutas TAK**, comparta el`.zip`A iTAK, y elija **Importar paquete de misión**.

> **Tip — ¿Dónde están mis rutas?**
> El`TAK Routes`La carpeta se crea la primera vez que llega una ruta. Si no lo ves, aún no se han recibido rutas. El KML dentro del zip es un KML 2.2 LineString estándar; también puede abrirlo en Google Earth o en cualquier visor de KML.

## Cómo funciona bajo el capó

No necesita configurar nada: la aplicación elige automáticamente el mejor formato de cable TAK que admite su radio. Firmware 2.8.0+ utiliza el nuevo formato V2 con compresión zstd-dictionary para tipos de mensajes más ricos y transmisiones LoRa más cortas. El firmware más antiguo sigue usando el formato V1 heredado, que lleva PLI y GeoChat entre dos nodos cualesquieras, además de un respaldo más rico de Apple a Apple para formas, marcadores y rutas.

Los desarrolladores y usuarios curiosos pueden leer todos los detalles del protocolo en[Protocolo TAK](../developer/tak-protocol.html).

## Localización y corrección de fallos

**El cliente de TAK no se conecta**
- Asegúrese de que el servidor TAK en la aplicación esté habilitado en **Configuración → Servidor TAK**.
- Confirme que su canal principal tiene un nombre no predeterminado **y** clave de cifrado; de lo contrario, el servidor se ejecuta en modo de solo lectura. Utilice **Fix Channel** en el banner de advertencia si se muestra.
- Para clientes mTLS, confirme que se importó un paquete P12 / PEM en **Certificados**.

**Las rutas no aparecen en iTAK**
- ITAK ignora la ruta CoT de la transmisión TCP a propósito. Abra el zip guardado desde **Archivos → Meshtastic → TAK Routes** e impórtelo como un paquete de misión.
- Si el`TAK Routes`Falta la carpeta, aún no ha llegado ninguna ruta CoT.

**Los selectores de identidad están deshabilitados**
- La radio debe informar de la configuración de su módulo TAK a la aplicación antes de que los selectores lo habiliten. Vuelve a conectarte si no llega en unos segundos.
- El nodo conectado debe tener el rol de dispositivo **TAK** o **TAK Tracker** - Equipo / Rol no tiene efecto en otros roles.

## Requisitos

- Firmware **2.3 o posterior** en su radio para TAK PLI / GeoChat básico; **2.8.0 o posterior** para el formato de cable TAK V2 completo.
- Una aplicación de cliente compatible con ATAK / iTAK / TAK en su teléfono o tableta.
- Dispositivo configurado con el rol **TAK** o **TAK Tracker**.

