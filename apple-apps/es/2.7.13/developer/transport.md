
# Capa de transporte

`AccessoryManager`Resume BLE, TCP/IP y transportes en serie detrás de una sola interfaz. Las vistas y los servicios interactúan solo con`AccessoryManager`- Nunca con implementaciones de transporte directamente.

## Implementaciones de transporte

El transporte vive en`Meshtastic/Accessory/Transports/`:

| Ficha | Protocolo de comunicación | Notas |
|------|----------|-------|
| `BLETransport.swift` | CoreBluetooth | Conexión BLE estándar a radios |
| `TCPTransport.swift` | Red.marco | Wi-Fi / TCP/IP a radios con redes |
| `SerialTransport.swift` | Serie de IOKit | Solo macOS; adaptadores USB-serie |

Cada transporte se ajusta a un`MeshTransport`Protocolo que expone`connect()`,`disconnect()`,`send(data:)`, Y un`received`Editor.

## Mapa de extensión de AccessoryManager

| Extensión | Métodos clave |
|-----------|------------|
| `+Discovery` | `startScanning()`, `stopScanning()`, `peripheral(_:didDiscover:)` |
| `+Connect` | `connect(peripheral:)`, `disconnect()`, `centralManager(_:didConnect:)` |
| `+ToRadio` | `sendPacket(_:)`, `sendWantConfig()`, `sendWaypoint(_:)` |
| `+FromRadio` | `handleFromRadio(_:)`, `handleMeshPacket(_:)` |
| `+Position` | `startLocationUpdates()`, `sendPosition(_:)` |
| `+MQTT` | `connectMQTT()`, `publishPacket(_:)`, `mqttClient(_:didReceiveMessage:)` |
| `+TAK` | `handleATAKPluginPacket(_:)`,`handleATAKPluginV2Packet(_:)`,`handleATAKForwarderPacket(_:)`,`sendTAKPacket(_:channel:)`,`sendTAKV2Packet(_:channel:)`,`sendCoTToMeshV2(_:channel:)`. Ver[Protocolo TAK](tak-protocol.html)Para el detalle del formato de cable V1/V2. |

## Flujo de paquetes (entrante)

```
Radio (BLE/TCP/Serial)
  → Transport.received publisher
  → AccessoryManager+FromRadio.handleFromRadio(_:)
  → Decode protobuf (MeshtasticProtobufs)
  → Route by packet type:
      MeshPacket  → handleMeshPacket(_:)
      NodeInfo    → updateNodeInfo(_:)
      MyNodeInfo  → updateMyNodeInfo(_:)
      Config      → updateConfig(_:)
      ...
  → Write to SwiftData via MeshPackets @ModelActor
  → Publish changes via @Published properties (UI updates)
```

## Flujo de paquetes (saliente)

```
View / Service
  → AccessoryManager+ToRadio.sendPacket(_:)
  → Encode to protobuf (ToRadio wrapper)
  → Transport.send(data:)
  → Radio
```

## Secuenciación de conexiones

`AccessoryManager+Connect`Ejecuta la configuración de la conexión como una serie secuenciada de pasos: conexión de transporte, latido del corazón,`wantConfig`, Recuperación opcional de la base de datos y comprobaciones de versión.

Durante un cambio de radio explícito desde la vista Conectar, la aplicación utiliza la misma canalización de conexión, pero permite una actualización posterior a la configuración adicional. Una vez`sendWantConfig()`Completa para el dispositivo recién seleccionado, la aplicación primero aplica el paquete`DeviceHardware.json`Catálogo e imágenes de dispositivos agrupados en SwiftData, luego programa`MeshtasticAPI.shared.refreshDevicesAPIData()`En el fondo. Esa actualización de red actualiza el mismo catálogo de hardware almacenado en caché localmente de`https://api.meshtastic.org/resource/deviceHardware`Sin bloquear el resto de la secuencia de conexión.

Esta actualización solo está habilitada para el flujo de radio conmutador. Las conexiones automáticas y las conexiones ordinarias continúan usando el apretón de manos de transporte estándar sin forzar una actualización del catálogo de hardware.

## Añadiendo un nuevo tipo de paquete

1. Añade la definición de protobuf en el`protobufs/`Submódulo.
2. Carrera`./scripts/gen_protos.sh`.
3. Añadir un caso de decodificación/envío en`AccessoryManager+FromRadio.handleFromRadio(_:)`.
4. Añade un método de envío en`AccessoryManager+ToRadio.swift`.
5. Agregue una propiedad de modelo o una entidad SwiftData si los datos necesitan persistir.
6. Escriba pruebas unitarias contra el viaje de ida y vuelta de codificación/decodificación.

## Notas de concurrencia

`AccessoryManager`No es`@MainActor`. Su`@Published`Las propiedades se observan desde las vistas de SwiftUI en el actor principal. Uso`await MainActor.run { }`Al actualizar las propiedades publicadas de tareas en segundo plano o devoluciones de llamada de delegados de CoreBluetooth.

Las escrituras de persistencia en segundo plano deben pasar por el`MeshPackets` `@ModelActor`, No el principal`ModelContext`.

