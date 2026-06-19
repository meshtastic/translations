
# Protocolo TAK

La aplicación implementa tres formatos de cable CoT (Cursor-on-Target) sobre LoRa: dos rutas V1 heredadas y una ruta V2 que utiliza el[TAKPacket-SDK](https://github.com/meshtastic/TAKPacket-SDK)Para la compresión del diccionario zstd y cargas útiles tipadas más ricas. Esta página documenta la elección del formato, el envío de recepción y la infraestructura de soporte.

## Formatos de cable

| Formato | Puerto | Carga | Usado cuando |
|--------|------|---------|-----------|
| V1 ATAK_PLUGIN | 72 | Desnudo`TAKPacket`Protobuf (solo PLI / GeoChat) | Firmware de radio conectado < 2.8.0 |
| V1 ATAK_FORWARDER | 257 | XML CoT comprimido en zlib, opcionalmente código fuente (LT) para cargas útiles de múltiples paquetes | Firmware de radio conectado < 2.8.0, tipo de CoT que no sea PLI / GeoChat (solo de Apple a Apple) |
| V2 ATAK_ENCHUFE_V2 | 78 | `TAKPacketV2`Protobuf, comprimido con el diccionario TAKPacket-SDK zstd | Firmware de radio conectado ≥ 2.8.0 |

V2 lleva el vocabulario completo de CoT tipado: PLI, GeoChat, formas, marcadores, rutas, casevac, emergencia y tarea. V1 ATAK_PLUGIN lleva solo PLI y GeoChat; todo lo demás se remue a la ruta V1 ATAK_FORWARDER.

## Horquilla por envío

`TAKMeshtasticBridge.sendToMesh(_:clientInfo:)`Elige el formato en cada envío basado en`AccessoryManager.supportsTAKv2`, Que comprueba la versión del firmware de la radio conectada:

```swift
if accessoryManager.supportsTAKv2 {
    // V2: SDK-driven path
    let parser = MeshtasticTAK.CotXmlParser()
    let packet = try parser.parse(strippedXml)
    let compressor = MeshtasticTAK.TakCompressor()
    // `compressWithRemarksFallback` returns `Data?` — `nil` means the
    // payload is still over the LoRa MTU even after `<remarks>` are
    // stripped. The real `sendCoTToMeshV2` translates that into a
    // thrown `AccessoryError.ioFailed(...)` so the caller's `do/catch`
    // doesn't treat the silent drop as a successful send.
    guard let wire = try compressor.compressWithRemarksFallback(packet, maxWireBytes: 225) else {
        throw AccessoryError.ioFailed("TAK V2 payload exceeds LoRa wire size limit")
    }
    try await sendTAKV2Packet(wire, channel: channel)
} else {
    // V1: classify, then dispatch
    switch GenericCoTHandler.shared.classifySendMethod(for: cot) {
    case .takPacketPLI, .takPacketChat:
        let pkt = convertToTAKPacket(cot: cot)
        try await sendTAKPacket(pkt, channel: channel)
    case .exiDirect, .exiFountain:
        try await GenericCoTHandler.shared.sendGenericCoT(cot, channel: channel)
    }
}
```

La bifurcación es por envío (no por sesión), por lo que una radio que actualiza a mitad de sesión salta a V2 inmediatamente.

## Recibir envío

`AccessoryManager.swift`El interruptor de portnum exhaustivo envía los paquetes TAK entrantes a los manejadores en`AccessoryManager+TAK.swift`:

| Portnum | Manipulador | Comportamiento |
|---------|---------|----------|
| `.atakPlugin`(72) | `handleATAKPluginPacket(_:)` | Decodificar desnudo`TAKPacket`Protobuf; convertir PLI / GeoChat a`CoTMessage`; Reenviar a los clientes de TAK a través de`TAKServerManager.shared.broadcast(_:)`. |
| `.atakPluginV2`(78) | `handleATAKPluginV2Packet(_:)` | Zstd-descomprimir con`TakCompressor`; Reconstruir CoT XML con`CotXmlBuilder`; Eliminar el prólogo XML y el espacio en blanco entre etiquetas; reenviar XML sin procesar a través de`broadcastRawXml(_:)`Así que el detalle de la forma (`<link point>`Vértices, colores, trazo) sobrevive. Ruta CoT (`b-m-r`) Activa el[Paquete de datos de ruta](#route-data-packages)Efecto secundario. |
| `.atakForwarder`(257) | `handleATAKForwarderPacket(_:)` | Entrega a`GenericCoTHandler.handleIncomingForwarderPacket(_:)`, Que vuelve a ensamblar los fragmentos de la fuente y zlib descomprime el CoT XML resultante antes de la emisión. |

## TAKPacket-SDK

El[TAKPacket-SDK](https://github.com/meshtastic/TAKPacket-SDK)El paquete Swift está anclado en`Meshtastic.xcworkspace/.../Package.resolved`. El puente utiliza tres API:

-`MeshtasticTAK.CotXmlParser().parse(_:)`— Analizar CoT XML en un`TAKPacketV2`Protobuf.`throws`; Las personas que llaman deben`try`.
-`MeshtasticTAK.TakCompressor().compressWithRemarksFallback(_:maxWireBytes:)`— Elige el diccionario zstd más adecuado para la carga útil, intenta la compresión y en el desbordamiento vuelve a intentarlo con`<remarks>`Despojado. Devoluciones`nil`Si la carga útil es demasiado grande incluso sin comentarios;`sendCoTToMeshV2`Traduce el`nil`En`AccessoryError.ioFailed(...)`Así que la llamada`do/catch`No trata la caída como un envío exitoso.
-`MeshtasticTAK.CotXmlBuilder().build(_:)`— Viajes de ida y vuelta`TAKPacketV2`De vuelta a CoT XML para reenviar a los clientes TAK.

La constante MTU del cable`maxWirePayloadBytes = 225`Refleja el presupuesto del marco LoRa después del sobre Meshtastic.

## Administrador de identidad

`TAKIdentitySection`(Incrustado en`TAKServerConfig`) Lee`node.takConfig`Para el equipo actual y los valores del rol. Cuando el nodo no tiene la configuración TAK almacenada en caché,`requestTakConfigIfNeeded()`Envía una solicitud de administrador a través de`AccessoryManager.requestTAKModuleConfig(fromUser:toUser:)`Para que los usuarios primerizos no vean un perma-spinner.

Guardar los despachos de identidad`AccessoryManager.saveTAKModuleConfig(config:fromUser:toUser:)`, Que empaqueta un`ModuleConfig.TAKConfig`Dentro de un`AdminMessage`Y lo envía al puerto de administración.

## Cola fuera de línea

`TAKServerManager`Almacena en búfer CoT saliente para la entrega cuando un cliente TAK se vuelve a conectar:

```swift
private enum QueuedPayload {
    case message(CoTMessage)
    case rawXml(String)
}
```

`broadcast(_:)`En colas`.message`Cargas útiles;`broadcastRawXml(_:)`En colas`.rawXml`Así que las formas / rutas / marcadores V2 conservan sus elementos de detalle. La cola tiene un TTL de 5 minutos y un límite de 50 entradas.`drainOfflineQueue()`Envía la ruta correcta por variante de carga útil cuando un cliente (re)se conecta.

## Paquetes de datos de ruta

`RouteDataPackageGenerator`(En`Meshtastic/Helpers/TAK/`) Convierte la ruta CoT (`b-m-r`) En paquetes de datos ATAK KML-inside-zip que el usuario puede descargar en iTAK (que ignora silenciosamente la ruta que CoT recibió a través de su conexión de transmisión TCP).

La tubería:

1.`generateKml(routeXml:)`Extractos`<event uid>`,`<contact callsign>`, Y cada`<link point="lat,lon,hae">`Waypoint vía`attributeValue(in:on:named:)`, Que admite atributos de comillas simples y dobles.
2.`sanitizeForFilename(_:)`Separadores de ruta de tiras, caracteres de control y`..`Secuencias de la ruta UID para que sea seguro usarla en los nombres de los archivos y la ruta del directorio temporal.`escapeXml(_:)`Se escapa por separado del valor antes de la interpolación en el manifiesto`value="..."`Atributo.
3.`generateDataPackage(routeXml:)`Escribe el KML y`manifest.xml`A un directorio temporal y los comprime con`NSFileCoordinator(readingItemAt:options:.forUploading)`.
4.`saveToDocuments(fileName:zipData:)`Escribe el zip a`Documents/TAK Routes/<sanitizedUid>.zip`(Creando el directorio en el primer uso).
5.`AccessoryManager+TAK.handleATAKPluginV2Packet(_:)`Publica un`Notification`Titulado **Ruta recibida** con el signo de llamada de la ruta como subtítulo y cuerpo **Guardado en Archivos → Meshtastic → Rutas TAK. Abrir en iTAK para importar. **

## Capacidades

`AccessoryManager.supportsTAKv2`Es la puerta canónica:

```swift
var supportsTAKv2: Bool { checkIsVersionSupported(forVersion: "2.8.0") }
```

Utilice esta propiedad (en lugar de analizar la versión del firmware en línea) en cualquier lugar donde se necesite una decisión V1/V2. Las futuras características de TAK SDK que requieren una versión de firmware más alta deben agregar una propiedad hermana con un corte claro para que el puente permanezca declarativo.

## Archivos relacionados

-`Meshtastic/Helpers/TAK/TAKMeshtasticBridge.swift`— Bifurcación V1/V2`sendToMesh`.
-`Meshtastic/Accessory/Accessory Manager/AccessoryManager+TAK.swift`— Enviar y recibir manejadores.
-`Meshtastic/Helpers/TAK/TAKServerManager.swift`— Servidor TCP, cola fuera de línea, gestión de certificados.
-`Meshtastic/Helpers/TAK/GenericCoTHandler.swift`— Clasificación V1 ATAK_FORWARDER y reensamblaje de la fuente.
-`Meshtastic/Helpers/TAK/RouteDataPackageGenerator.swift`— Escritor de paquetes de datos KML.
-`Meshtastic/Views/Settings/TAKServerConfig.swift`— Combinada la pantalla de configuración del servidor TAK con el integrado`TAKIdentitySection`.

Ver también[Capa de transporte](transport.html)Para el mapa de extensión de AccessoryManager.

