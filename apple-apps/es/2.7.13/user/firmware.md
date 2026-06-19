
# Actualizaciones de firmware

La aplicación puede comprobar e instalar actualizaciones de firmware Meshtastic directamente en su radio conectada a través de Bluetooth.

## Comprobación de actualizaciones

1. Conéctate a tu radio.
2. Vaya a **Configuración → Actualizaciones de firmware**.
3. La aplicación muestra la versión del firmware que se está ejecutando actualmente en su radio y la última versión estable disponible en GitHub.

## Instalación de una actualización

1. Toque **Actualizar firmware** cuando haya una versión más reciente disponible.
2. La aplicación descarga el binario de firmware correcto para su hardware.
3. La radio entra en modo de actualización (DFU) y el nuevo firmware se transfiere a través de BLE.
4. La radio se reinicia automáticamente cuando se completa la actualización.

| Icono | Progreso | Descripción |
|------|----------|-------------|
| ![0%](../assets/screenshots/progressZero.png) | Comenzando | Iniciando la actualización: descarga binaria de firmware. |
| ![50%](../assets/screenshots/progressHalf.png) | En curso | Transferencia de firmware en curso a través de BLE. |
| ![Complete](../assets/screenshots/progressComplete.png) | Completo | Transferencia finalizada - la radio se está reiniciando. |
| ![Error](../assets/screenshots/progressError.png) | Error informático | La actualización falló: consulte Solución de problemas a continuación. |

**No cierre la aplicación ni salga del alcance de Bluetooth durante una actualización de firmware. **

## Actualizar canales

| Canal | Descripción |
|---------|-------------|
| Estable | Recomendado para la mayoría de los usuarios. Lanzamientos probados. |
| Alfa | Acceso anticipado: puede contener errores. Usar solo en dispositivos secundarios/de prueba. |

Seleccione el canal de actualización en **Configuración → Configuración de la aplicación → Canal de firmware**.

## Localización y corrección de fallos

**La actualización falla a mitad de camino**
- Mantenga la radio dentro de 1-2 metros de su teléfono durante la actualización.
- Si la radio aparece bloqueada después de una actualización fallida, generalmente se puede recuperar usando el[Intermitente Meshtastic](https://flasher.meshtastic.org/)En un ordenador.

![Incompatible firmware version warning](../assets/screenshots/invalidVersion.png)

![Security update recommended](../assets/screenshots/securityVersionNag.png)

**La radio no aparece en la lista de firmware**
- La función de actualización de firmware requiere una radio conectada (BLE o TCP).
- Algunas radios más antiguas no admiten actualizaciones OTA. Revisa el[Lista de compatibilidad de hardware](https://meshtastic.org/docs/hardware/).

**La versión se muestra como desconocida**
- Asegúrese de que la radio esté completamente conectada y sincronizada (generalmente toma de 5 a 10 segundos después de que se establezca la conexión BLE).

