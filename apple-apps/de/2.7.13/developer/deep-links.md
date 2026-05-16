
# Tiefe Links

Die App registriert die`meshtastic:///`URL-Schema. Nutzung`Router.route(url:)`Um eingehende URLs zu verarbeiten. Deep Links funktionieren von Safari, Shortcuts, Siri Intents und anderen Apps.

## Hinzufügen eines neuen Deep Links

1. Fügen Sie einen Fall zu den entsprechenden`*NavigationState`Aufläumen in`Meshtastic/Router/NavigationState.swift`.
2. Auf den neuesten Stand bringen`Router`'S Routing-Helfer in`Meshtastic/Router/Router.swift`.
3. Dokumentieren Sie die URL in der folgenden Tabelle.

## Nachrichten

| URL | Beschreibung |
|-----|-------------|
| [`meshtastic:///messages`](meshtastic:///Nachrichten) | Registerkarte Nachrichten |
| `meshtastic:///messages?channelId={channelId}&messageId={messageId}` | Kanalnachrichten (`messageId`Ist optional) |
| `meshtastic:///messages?userNum={userNum}&messageId={messageId}` | Direktnachrichten (`messageId`Ist optional) |

## Verbinden

| URL | Beschreibung |
|-----|-------------|
| [`meshtastic:///connect`](Meshtastic:///verbinden) | Registerkarte Verbinden |

## Knoten

| URL | Beschreibung |
|-----|-------------|
| [`meshtastic:///nodes`](Meshtastic:///nodes) | Registerkarte Knoten |
| `meshtastic:///nodes?nodenum={nodenum}` | Ausgewählter Knoten |

## Netzkarte

| URL | Beschreibung |
|-----|-------------|
| [`meshtastic:///map`](Meshtastic:///karte) | Karten-Tab |
| `meshtastic:///map?nodenum={nodenum}` | Knoten auf der Karte |
| `meshtastic:///map?waypointId={waypointId}` | Wegpunkt auf der Karte |

## Einstellungen

Für Einstellungs-URLs werden keine Parameter unterstützt.

| URL | Beschreibung |
|-----|-------------|
| [`meshtastic:///settings/about`](meshtastic:///Einstellungen/über) | Über Meshtastic |
| [`meshtastic:///settings/appSettings`](meshtastic:///Einstellungen/appEinstellungen) | App-Einstellungen |
| [`meshtastic:///settings/helpDocs`](meshtastic:///Einstellungen/helpDocs) | Hilfe & Dokumentation |
| [`meshtastic:///settings/routes`](meshtastic:///Einstellungen/Routen) | Routen |
| [`meshtastic:///settings/routeRecorder`](meshtastic:///Einstellungen/RouteRecorder) | Routenrekorder |
| **Funkkonfiguration** |  |
| [`meshtastic:///settings/lora`](meshtastic:///Einstellungen/lora) | LoRa-Konfiguration |
| [`meshtastic:///settings/channels`](meshtastic:///Einstellungen/Kanäle) | Sender |
| [`meshtastic:///settings/security`](meshtastic:///Einstellungen/Sicherheit) | Sicherheitskonfiguration |
| [`meshtastic:///settings/shareQRCode`](meshtastic:///settings/shareQRCode) | QR-Code teilen |
| **Gerätekonfiguration** |  |
| [`meshtastic:///settings/user`](meshtastic:///Einstellungen/Benutzer) | Benutzerkonfiguration |
| [`meshtastic:///settings/bluetooth`](meshtastic:///Einstellungen/bluetooth) | Bluetooth-Konfiguration |
| [`meshtastic:///settings/device`](meshtastic:///Einstellungen/Gerät) | Gerätekonfiguration |
| [`meshtastic:///settings/display`](meshtastic:///Einstellungen/Anzeige) | Anzeigekonfiguration |
| [`meshtastic:///settings/network`](meshtastic:///Einstellungen/Netzwerk) | Netzwerkkonfiguration |
| [`meshtastic:///settings/position`](meshtastic:///Einstellungen/Position) | Positionskonfiguration |
| [`meshtastic:///settings/power`](meshtastic:///Einstellungen/Leistung) | Energiekonfiguration |
| **Modulkonfiguration** |  |
| [`meshtastic:///settings/ambientLighting`](meshtastic:///settings/ambientLighting) | Umgebungsbeleuchtung |
| [`meshtastic:///settings/cannedMessages`](meshtastic:///Einstellungen/cannedMessages) | Einmundbotschaften |
| [`meshtastic:///settings/detectionSensor`](meshtastic:///settings/detectionSensor) | Erkennungssensor |
| [`meshtastic:///settings/externalNotification`](meshtastic:///Einstellungen/externeBenachrichtigung) | Externe Benachrichtigung |
| [`meshtastic:///settings/mqtt`](meshtastic:///Einstellungen/mqtt) | MQTT |
| [`meshtastic:///settings/paxCounter`](meshtastic:///Einstellungen/paxCounter) | Pax-Zähler |
| [`meshtastic:///settings/rangeTest`](meshtastic:///settings/rangeTest) | Entfernungstest |
| [`meshtastic:///settings/ringtone`](meshtastic:///Einstellungen/Klingelton) | Klingelton |
| [`meshtastic:///settings/serial`](meshtastic:///Einstellungen/seriell) | Serie |
| [`meshtastic:///settings/storeAndForward`](meshtastic:///settings/storeAndForward) | Speichern & Weiterleiten |
| [`meshtastic:///settings/telemetry`](meshtastic:///Einstellungen/Telemetrie) | Telemetrie |
| **TAK** |  |
| [`meshtastic:///settings/tak`](Meshtastic:///settings/tak) | TAK-Konfiguration |
| **Protokollierung** |  |
| [`meshtastic:///settings/debugLogs`](meshtastic:///Einstellungen/DebugLogs) | Protokolle Debuggen |
| **Entwickler** |  |
| [`meshtastic:///settings/appFiles`](meshtastic:///einstellungen/appDateien) | App-Dateien |
| [`meshtastic:///settings/tools`](meshtastic:///Einstellungen/Werkzeuge) | Werkzeuge (iOS 18+) |
| [`meshtastic:///settings/coreDataBrowser`](meshtastic:///settings/coreDataBrowser) | Datenbrowser (nur DEBUG) |
| [`meshtastic:///settings/firmwareUpdates`](meshtastic:///settings/firmwareUpdates) | Firmware-Updates |

