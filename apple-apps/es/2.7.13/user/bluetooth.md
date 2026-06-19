
# Conexión del dispositivo Bluetooth

La aplicación Meshtastic se conecta a su radio a través de Bluetooth Low Energy (BLE). Puede administrar varias radios y cambiar entre ellas sin volver a emparejarlas.

## Conexión de una radio

1. Enciende tu radio Meshtastic.
2. Abre la aplicación y toca la pestaña **Conectar**.
3. La aplicación escanea los dispositivos cercanos automáticamente cuando no estás conectado.
4. Toque el nombre de su dispositivo en la lista para conectarse.

La aplicación recuerda su dispositivo preferido y se vuelve a conectar automáticamente cuando la radio está dentro del alcance.

## Desconectar una radio

Desliza el dedo hacia la izquierda en una radio conectada en la vista Conectar y pulsa **Desconectar**. La radio sigue funcionando en la malla, simplemente deja de sincronizarse con la aplicación.

## Actividad en vivo

Presione larga una fila de radio conectada para iniciar una actividad en vivo (iOS 16.2+). La actividad en vivo muestra el estado de la malla en su pantalla de bloqueo y en la isla dinámica.

## Gestión de múltiples radios

Puedes emparejar varias radios, pero solo una está activa a la vez. Cambie entre ellos tocando un dispositivo diferente en la vista Conectar.

Cuando cambias de radio, la aplicación restaura la última base de datos local guardada para esa radio si existe una, luego se vuelve a conectar y reanuda la sincronización con el dispositivo recién activo. Después de que la nueva radio termine su apretón de manos de configuración inicial, la aplicación primero vuelve a aplicar el catálogo de hardware Meshtastic incluido que se envía con la aplicación, luego actualiza el mismo catálogo de la API de Meshtastic en segundo plano para que los nombres de hardware, las imágenes y los metadatos de destino de firmware permanezcan actualizados.

## Intensidad de la señal BLE

La aplicación muestra la intensidad de la señal Bluetooth de los dispositivos cercanos durante el escaneo:

![BLE Signal Strength](../assets/screenshots/bleSignalStrength.png)

## Iconos de estado de conexión

| Icono | Significado |
|------|---------|
| ![BLE connected](../assets/screenshots/btConnected.png) | Conectado a través de BLE |
| ![Reconnecting](../assets/screenshots/btReconnecting.png) | Reconexión / reintento |
| ![TCP connected](../assets/screenshots/tcpConnected.png) | Conectado a través de TCP/IP |
| ![Serial connected](../assets/screenshots/serialConnected.png) | Conectado a través del número de serie (macOS) |

## Localización y corrección de fallos

**La radio no aparece en la lista**
- Asegúrate de que Bluetooth esté activado en Ajustes de iOS → Bluetooth.
- Muévete a menos de 10 metros de la radio.
- Reinicia la radio.
- La aplicación escucha continuamente los anuncios de BLE; las radios cercanas deberían aparecer en unos pocos segundos. Si un dispositivo desaparece de la lista, reaparecerá automáticamente la próxima vez que se escuche.

**La conexión se cae repetidamente**
- Comprueba el nivel de la batería de la radio.
- Intenta olvidar el dispositivo en Configuración de iOS → Bluetooth y volver a conectar.

**La aplicación pide permiso de Bluetooth**
- Concede permiso en Configuración de iOS → Privacidad y Seguridad → Bluetooth → Meshtastic.

---

> **Tip — Radio conectada**
> Desliza el dedo hacia la izquierda para desconectar. Presione largamente para iniciar la actividad en vivo.

