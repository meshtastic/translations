
# Enlaces profundos

La aplicación registra el`meshtastic:///`Esquema de URL. Uso`Router.route(url:)`Para manejar las URL entrantes. Los enlaces profundos funcionan desde Safari, Shortcuts, Siri Intents y otras aplicaciones.

## Añadiendo un nuevo enlace profundo

1. Añade un caso al apropiado`*NavigationState`Enumerar en`Meshtastic/Router/NavigationState.swift`.
2. Actualización`Router`Los ayudantes de enrutamiento en`Meshtastic/Router/Router.swift`.
3. Documente la URL en la siguiente tabla.

## Mensajes

| URL | Descripción |
|-----|-------------|
| [`meshtastic:///messages`](meshtastic:///messages) | Pestaña de mensajes |
| `meshtastic:///¿mensajes? channelId={channelId}&messageId={messageId}` | Mensajes de canal (`messageId` es opcional) |
| `meshtastic:///¿mensajes? UserNum={userNum}&messageId={messageId}` | Mensajes directos (`messageId` es opcional) |

## Conectar

| URL | Descripción |
|-----|-------------|
| [`meshtastic:///connect`](meshtastic:///connect) | Pestaña de conexión |

## Nodos

| URL | Descripción |
|-----|-------------|
| [`meshtastic:///nodes`](meshtastic:///nodes) | Pestaña de nodos |
| `meshtastic:///nodes? Nodenum={nodenum}` | Nodo seleccionado |

## Mapa de malla

| URL | Descripción |
|-----|-------------|
| [`meshtastic:///map`](meshtastic:///map) | Pestaña de mapa |
| `meshtastic:///mapa? Nodenum={nodenum}` | Nodo en el mapa |
| `meshtastic:///mapa? waypointId={waypointId}` | Waypoint en el mapa |

## Ajustes

No se admiten parámetros para las URL de configuración.

| URL | Descripción |
|-----|-------------|
| [`meshtastic:///settings/about`](meshtastic:///settings/about) | Acerca de Meshtastic |
| [`meshtastic:///settings/appSettings`](meshtastic:///settings/appSettings) | Configuración de la aplicación |
| [`meshtastic:///settings/helpDocs`](meshtastic:///settings/helpDocs) | Ayuda y documentación |
| [`meshtastic:///settings/routes`](meshtastic:///settings/routes) | Rutas |
| [`meshtastic:///settings/routeRecorder`](meshtastic:///settings/routeRecorder) | Registrador De Rutas |
| **Config. de radio** |  |
| [`meshtastic:///settings/lora`](meshtastic:///settings/lora) | Configuración LoRa |
| [`meshtastic:///settings/channels`](meshtastic:///settings/channels) | Canal |
| [`meshtastic:///settings/security`](meshtastic:///settings/security) | Configuración de seguridad |
| [`meshtastic:///settings/shareQRCode`](meshtastic:///settings/shareQRCode) | Compartir código QR |
| **Configurar el dispositivo** |  |
| [`meshtastic:///settings/user`](meshtastic:///settings/user) | Configuración del usuario |
| [`meshtastic:///settings/bluetooth`](meshtastic:///settings/bluetooth) | Configuración de Bluetooth |
| [`meshtastic:///settings/device`](meshtastic:///settings/device) | Configuración del dispositivo |
| [`meshtastic:///settings/display`](meshtastic:///settings/display) | Configuración de visualización |
| [`meshtastic:///settings/network`](meshtastic:///settings/network) | Configuración de red |
| [`meshtastic:///settings/position`](meshtastic:///settings/position) | Configuración de posición |
| [`meshtastic:///settings/power`](meshtastic:///settings/power) | Configuración de energía |
| **Configuración del módulo** |  |
| [`meshtastic:///settings/ambientLighting`](meshtastic:///settings/ambientLighting) | Iluminación ambiental |
| [`meshtastic:///settings/cannedMessages`](meshtastic:///settings/cannedMessages) | Mensajes enlatados |
| [`meshtastic:///settings/detectionSensor`](meshtastic:///settings/detectionSensor) | Sensor de detección |
| [`meshtastic:///settings/externalNotification`](meshtastic:///settings/externalNotification) | Notificación externa |
| [`meshtastic:///settings/mqtt`](meshtastic:///settings/mqtt) | MQTT |
| [`meshtastic:///settings/paxCounter`](meshtastic:///settings/paxCounter) | Contador de Pax |
| [`meshtastic:///settings/rangeTest`](meshtastic:///settings/rangeTest) | Prueba de rango |
| [`meshtastic:///settings/ringtone`](meshtastic:///settings/ringtone) | Tono de llamada |
| [`meshtastic:///settings/serial`](meshtastic:///settings/serial) | Serie |
| [`meshtastic:///settings/storeAndForward`](meshtastic:///settings/storeAndForward) | Almacenar y reenviar |
| [`meshtastic:///settings/telemetry`](meshtastic:///settings/telemetry) | Telemetría |
| **TOQUE** |  |
| [`meshtastic:///settings/tak`](meshtastic:///settings/tak) | Configuración TAK |
| **Inicio de sesión** |  |
| [`meshtastic:///settings/debugLogs`](meshtastic:///settings/debugLogs) | Registros de depuración |
| **Desarrolladores** |  |
| [`meshtastic:///settings/appFiles`](meshtastic:///settings/appFiles) | Archivos de aplicaciones |
| [`meshtastic:///settings/tools`](meshtastic:///settings/tools) | Herramientas (iOS 18+) |
| [`meshtastic:///settings/coreDataBrowser`](meshtastic:///settings/coreDataBrowser) | Navegador de datos (solo DEBUG) |
| [`meshtastic:///settings/firmwareUpdates`](meshtastic:///settings/firmwareUpdates) | Actualizaciones de firmware |

