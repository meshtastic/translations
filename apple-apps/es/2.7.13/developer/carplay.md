
# Arquitectura de CarPlay

Esta página cubre la implementación de la función CarPlay. Para la guía orientada al usuario, consulte [CarPlay](../user/carplay.md).

## Componentes

| Componente | Ficha | Descripción |
|---|---|---|
| `CarPlaySceneDelegate` | `Meshtastic/CarPlay/CarPlaySceneDelegate.swift` | `CPTemplateApplicationSceneDelegate`Que construye y administra la interfaz de usuario de dos pestañas |
| `CarPlayIntentDonation` | `Meshtastic/CarPlay/CarPlayIntentDonation.swift` | Donaciones entrantes y salientes`INSendMessageIntent`Interacciones para que las conversaciones aparezcan en los mensajes de CarPlay y Siri pueda leerlas en voz alta |
| `SendMessageIntentHandler` | `Meshtastic/Intents/SendMessageIntentHandler.swift` | Manijas`INSendMessageIntent`— Resuelve destinatarios/canales y envía el mensaje a través del transporte activo |
| `SearchForMessagesIntentHandler` | `Meshtastic/Intents/SearchForMessagesIntentHandler.swift` | Manijas`INSearchForMessagesIntent` |
| `SetMessageAttributeIntentHandler` | `Meshtastic/Intents/SetMessageAttributeIntentHandler.swift` | Manijas`INSetMessageAttributeIntent`(Marcar como leído) |
| `IntentHandler` | `Meshtastic/Intents/IntentHandler.swift` | Rutas`INIntent`S al manejador apropiado |

## Actualizaciones de plantillas

El delegado de la escena se suscribe a`AccessoryManager.shared.$isConnected`Con un **300 ms debounce** y llamadas`updateSections(_:)`En existente`CPListTemplate`Instancias en lugar de reconstruir todo el árbol de plantillas. Esto minimiza el parpadeo durante las reconexiones y evita activar el límite de velocidad de CarPlay en el reemplazo de la plantilla.

## Deduplicación de donación intencional

Las donaciones de intención se deduplican por sesión de CarPlay usando una memoria en memoria`Set`. Esto evita llamadas IPC repetidas al demonio de intenciones en cada actualización de lista (lo que ocurre en un temporizador mientras CarPlay está conectado).

Cuando comienza una nueva sesión de CarPlay, el conjunto se borra y hasta 50 mensajes no leídos se donan por lotes para que Siri pueda leerlos bajo demanda.

## Añadiendo una nueva intención

1. Crear un controlador en`Meshtastic/Intents/`De acuerdo con lo apropiado`INIntent`Protocolo.
2. Registrar al manejador en`IntentHandler.swift`'S`handler(for:)`Cambiar.
3. Declara la intención en`Meshtastic.entitlements`Bajo`com.apple.developer.siri`.
4. Añade una descripción de uso en`Info.plist`Si la intención requiere un nuevo permiso de privacidad.

