
# Bluetooth Device Connection

The Meshtastic app connects to your radio over Bluetooth Low Energy (BLE). You can manage multiple radios and switch between them without re-pairing.

## Connecting a Radio

1. Power on your Meshtastic radio.
2. Open the app and tap the **Connect** tab.
3. The app scans for nearby devices automatically when you are not connected.
4. Tap your device name in the list to connect.

The app remembers your preferred device and reconnects automatically when the radio is in range.

## Disconnecting a Radio

Desliza el dedo hacia la izquierda en una radio conectada en la vista Conectar y pulsa **Desconectar**. La radio sigue funcionando en la malla, simplemente deja de sincronizarse con la aplicación.

## Actividad en vivo

Presione larga una fila de radio conectada para iniciar una actividad en vivo (iOS 16.2+). La actividad en vivo muestra el estado de la malla en su pantalla de bloqueo y en la isla dinámica.

## Gestión de múltiples radios

Puedes emparejar varias radios, pero solo una está activa a la vez. Cambie entre ellos tocando un dispositivo diferente en la vista Conectar.

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

**La conexión se cae repetidamente**
- Comprueba el nivel de la batería de la radio.
- Intenta olvidar el dispositivo en Configuración de iOS → Bluetooth y volver a conectar.

**La aplicación pide permiso de Bluetooth**
- Concede permiso en Configuración de iOS → Privacidad y Seguridad → Bluetooth → Meshtastic.

---

> **Tip — Radio conectada**
> Desliza el dedo hacia la izquierda para desconectar. Presione largamente para iniciar la actividad en vivo.

