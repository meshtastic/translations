
# Conexión del dispositivo Bluetooth

La aplicación Meshtastic se conecta a su radio a través de Bluetooth Low Energy (BLE). Puede administrar múltiples radios y cambiar entre ellas sin volver a emparejarlas.

## Conectando una radio

1. Encienda su radio Meshtastic.
2. Abre la aplicación y toca la pestaña **Conectar**.
3. La aplicación escanea automáticamente los dispositivos cercanos cuando no estás conectado.
4. Toca el nombre de tu dispositivo en la lista para conectarte.

La aplicación recuerda tu dispositivo preferido y se reconecta automáticamente cuando la radio está en rango.

## Desconectando una radio

Desliza hacia la izquierda en una radio conectada en la vista Conectar y toca **Desconectar**. La radio continúa funcionando en la red inalámbrica, solo deja de sincronizarse con la aplicación.

## Actividad en vivo

Pulse y mantenga presionado durante mucho tiempo una fila de radios conectadas para iniciar una Actividad en Vivo (iOS 16.2+). La Actividad en Vivo muestra el estado de la red en su Pantalla de Bloqueo y en la Isla Dinámica.

## Administrar múltiples radios

Puedes emparejar múltiples radios, pero solo una está activa a la vez. Cambia entre ellas tocando un dispositivo diferente en la vista Conectar.

## Potencia del señal BLE

La aplicación muestra la potencia del señal Bluetooth de los dispositivos cercanos durante la escaneo:

![BLE Signal Strength](../assets/screenshots/bleSignalStrength.png)

## Iconos de estado de conexión

| icono | significado |
|------|---------|
| ![BLE connected](../assets/screenshots/btConnected.png) | Conectado a través de BLE |
| ![Reconnecting](../assets/screenshots/btReconnecting.png) | Reconectando / intentando de nuevo |
| ![TCP connected](../assets/screenshots/tcpConnected.png) | Conectado a través de TCP/IP |
| ![Serial connected](../assets/screenshots/serialConnected.png) | Conectado por serie (macOS) |

## localización y corrección de fallos

**Radio no aparece en la lista**
- Asegúrese de que Bluetooth esté habilitado en Ajustes de iOS → Bluetooth.
- Mueve dentro de 10 metros del radio.
- Reinicia la radio.
- La aplicación escucha continuamente anuncios BLE: las radios cercanas deberían aparecer en unos segundos. Si un dispositivo desaparece de la lista, volverá a aparecer automáticamente cuando se oiga la próxima vez.

**La conexión se pierde repetidamente**
- Comprueba el nivel de la batería en el radio.
- Intenta olvidarte del dispositivo en Ajustes de iOS → Bluetooth y volver a conectarte.

**La aplicación solicita permiso para Bluetooth**
- Dar permiso en Ajustes de iOS → Privacidad y seguridad → Bluetooth → Meshtastic.

---

> **Tip — Radio conectada**
> Desliza hacia la izquierda para desconectar. Mantén presionado para iniciar la actividad en vivo.

