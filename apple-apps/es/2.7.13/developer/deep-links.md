
# Enlaces profundos

La aplicación registra el`meshtastic:///`Esquema de URL. Uso`Router.route(url:)`Para manejar las URL entrantes. Los enlaces profundos funcionan desde Safari, Shortcuts, Siri Intents y otras aplicaciones.

## Añadiendo un nuevo enlace profundo

1. Añade un caso al apropiado`*NavigationState`Enumerar en`Meshtastic/Router/NavigationState.swift`.
2. Actualización`Router`Los ayudantes de enrutamiento en`Meshtastic/Router/Router.swift`.
3. Documente la URL en la siguiente tabla.

## Mensajes

| URL | Descripción |
|-----|-------------|
| [`meshtastic:///messages`](Meshtastic:///mensajes) | Pestaña de mensajes |
| `meshtastic:///messages?channelId={channelId}&messageId={messageId}` | Mensajes de canal (`messageId`Es opcional) |
| `meshtastic:///messages?userNum={userNum}&messageId={messageId}` | Mensajes directos (`messageId`Es opcional) |

## Conectar

| URL | Descripción |
|-----|-------------|
| [`meshtastic:///connect`](Meshtastic:///conectar) | Pestaña de conexión |

## Nodos

| URL | Descripción |
|-----|-------------|
| [`meshtastic:///nodes`](Meshtastic:///nodos) | Pestaña de nodos |
| `meshtastic:///nodes?nodenum={nodenum}` | Nodo seleccionado |

## Mapa

| URL | Descripción |
|-----|-------------|
| [`meshtastic:///map`](Meshtastic:///map) | Pestaña de mapa |
| `meshtastic:///map?nodenum={nodenum}` | Nodo en el mapa |
| `meshtastic:///map?waypointId={waypointId}` | Waypoint en el mapa |

## Ajustes

No se admiten parámetros para las URL de configuración.

| URL | Descripción |
|-----|-------------|
| [`meshtastic:///settings/about`](Meshtastic:///settings/about) | Acerca de Meshtastic |
| [`meshtastic:///settings/appSettings`](meshtastic:///settings/appSettings) | Configuración de la aplicación |
| [`meshtastic:///settings/helpDocs`](meshtastic:///settings/helpDocs) | Ayuda y documentación |
| [`meshtastic:///settings/routes`](Meshtastic:///settings/routes) | Rutas |
| [`meshtastic:///settings/routeRecorder`](meshtastic:///settings/routeRecorder) | Registrador De Rutas |
| **Config. de radio** |  |
| [`meshtastic:///settings/lora`](Meshtastic:///settings/lora) | Configuración LoRa |
| [`meshtastic:///settings/channels`](Meshtastic:///settings/channels) | Canal |
| [`meshtastic:///settings/security`](Meshtastic:///settings/security) | Configuración de seguridad |
| [`meshtastic:///settings/shareQRCode`](meshtastic:///configuración/shareQRCode) | Compartir código QR |
| **Configurar el dispositivo** |  |
| [`meshtastic:///settings/user`](Meshtastic:///configuraciones/usuario) | Configuración del usuario |
| [`meshtastic:///settings/bluetooth`](Meshtastic:///settings/bluetooth) | Configuración de Bluetooth |
| [`meshtastic:///settings/device`](Meshtastic:///settings/device) | Configuración del dispositivo |
| [`meshtastic:///settings/display`](Meshtastic:///settings/display) | Configuración de visualización |
| [`meshtastic:///settings/network`](Meshtastic:///settings/network) | Configuración de red |
| [`meshtastic:///settings/position`](Meshtastic:///settings/position) | Configuración de posición |
| [`meshtastic:///settings/power`](Meshtastic:///settings/power) | Configuración de energía |
| **Configuración del módulo** |  |
| [`meshtastic:///settings/ambientLighting`](meshtastic:///settings/ambientLighting) | Iluminación ambiental |
| [`meshtastic:///settings/audio`](Meshtastic:///configuraciones/audio) | Audio (Codec2, requiere región LORA_24) |
| [`meshtastic:///settings/cannedMessages`](meshtastic:///settings/cannedMessages) | Mensajes enlatados |
| [`meshtastic:///settings/detectionSensor`](meshtastic:///settings/detectionSensor) | Sensor de detección |
| [`meshtastic:///settings/externalNotification`](meshtastic:///settings/externalNotification) | Notificación externa |
| [`meshtastic:///settings/mqtt`](Meshtastic:///settings/mqtt) | MQTT |
| [`meshtastic:///settings/neighborInfo`](meshtastic:///settings/neighborInfo) | Información del vecino |
| [`meshtastic:///settings/paxCounter`](meshtastic:///settings/paxCounter) | Contador de Pax |
| [`meshtastic:///settings/rangeTest`](meshtastic:///settings/rangeTest) | Prueba de rango |
| [`meshtastic:///settings/ringtone`](Meshtastic:///settings/ringtone) | Tono de llamada |
| [`meshtastic:///settings/serial`](Meshtastic:///settings/serial) | Serie |
| [`meshtastic:///settings/statusMessage`](meshtastic:///settings/statusMessage) | Mensaje de estado |
| [`meshtastic:///settings/storeAndForward`](meshtastic:///settings/storeAndForward) | Almacenar y reenviar |
| [`meshtastic:///settings/telemetry`](Meshtastic:///settings/telemetry) | Telemetría |
| **TOQUE** |  |
| [`meshtastic:///settings/tak`](Meshtastic:///settings/tak) | Configuración TAK |
| **Inicio de sesión** |  |
| [`meshtastic:///settings/debugLogs`](meshtastic:///settings/debugLogs) | Registros de depuración |
| **Desarrolladores** |  |
| [`meshtastic:///settings/appFiles`](meshtastic:///settings/appFiles) | Archivos de aplicaciones |
| [`meshtastic:///settings/tools`](Meshtastic:///configuraciones/herramientas) | Herramientas (iOS 18+) |
| [`meshtastic:///settings/coreDataBrowser`](meshtastic:///settings/coreDataBrowser) | Navegador de datos (solo DEBUG) |
| [`meshtastic:///settings/firmwareUpdates`](meshtastic:///settings/firmwareUpdates) | Actualizaciones de firmware |

