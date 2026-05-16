
# Tiefe Links
Die App registriert die`meshtastic:///`URL-Schema. Nutzung`Router.route(url:)`Um eingehende URLs zu verarbeiten. Deep Links funktionieren von Safari, Shortcuts, Siri Intents und anderen Apps.
## Hinzufügen eines neuen Deep Links
1. Fügen Sie einen Fall zu den entsprechenden`*NavigationState`Aufläumen in`Meshtastic/Router/NavigationState.swift`.2. Auf den neuesten Stand bringen`Router`'S Routing-Helfer in`Meshtastic/Router/Router.swift`.3. Dokumentieren Sie die URL in der folgenden Tabelle.
## Nachrichten
| URL | Beschreibung |
|-----|-------------|
| [`meshtastic:///messages`](meshtastic:///messages) | Registerkarte Nachrichten |
| `meshtastic:///Nachrichten? Kanal-ID={Kanal-ID}&Nachrichten-ID={Nachrichten-ID}` | Kanalnachrichten (`messageId` ist optional) |
| `meshtastic:///Nachrichten? BenutzerNum={BenutzerNum}&Nachrichten-ID={Nachrichten-ID}` | Direktnachrichten (`messageId` ist optional) |

## Verbinden
| URL | Beschreibung |
|-----|-------------|
| [`meshtastic:///connect`](meshtastic:///connect) | Registerkarte Verbinden |

## Knoten
| URL | Beschreibung |
|-----|-------------|
| [`meshtastic:///nodes`](meshtastic:///nodes) | Registerkarte Knoten |
| `meshtastic:///knoten? Nodenum={nodenum}` | Ausgewählter Knoten |

## Netzkarte
| URL | Beschreibung |
|-----|-------------|
| [`meshtastic:///map`](meshtastic:///map) | Karten-Tab |
| `meshtastic:///map? Nodenum={nodenum}` | Knoten auf der Karte |
| `meshtastic:///map? WegpunktId={WegpunktId}` | Wegpunkt auf der Karte |

## Einstellungen
Für Einstellungs-URLs werden keine Parameter unterstützt.
| URL | Beschreibung |
|-----|-------------|
| [`meshtastic:///settings/about`](meshtastic:///settings/about) | Über Meshtastic |
| [`meshtastic:///settings/appSettings`](meshtastic:///settings/appSettings) | App-Einstellungen |
| [`meshtastic:///settings/helpDocs`](meshtastic:///settings/helpDocs) | Hilfe & Dokumentation |
| [`meshtastic:///Einstellungen/Routen`](meshtastic:///Einstellungen/Routen) | Routen |
| [`meshtastic:///settings/routeRecorder`](meshtastic:///settings/routeRecorder) | Routenrekorder |
| **Funkkonfiguration** |  |
| [`meshtastic:///settings/lora`](meshtastic:///settings/lora) | LoRa-Konfiguration |
| [`meshtastic:///settings/channels`](meshtastic:///settings/channels) | Sender |
| [`meshtastic:///Einstellungen/Sicherheit`](meshtastic:///Einstellungen/Sicherheit) | Sicherheitskonfiguration |
| [`meshtastic:///settings/shareQRCode`](meshtastic:///settings/shareQRCode) | QR-Code teilen |
| **Gerätekonfiguration** |  |
| [`meshtastic:///Einstellungen/Benutzer`](meshtastic:///Einstellungen/Benutzer) | Benutzerkonfiguration |
| [`meshtastic:///settings/bluetooth`](meshtastic:///settings/bluetooth) | Bluetooth-Konfiguration |
| [`meshtastic:///Einstellungen/Gerät`](meshtastic:///Einstellungen/Gerät) | Gerätekonfiguration |
| [`meshtastic:///settings/display`](meshtastic:///settings/display) | Anzeigekonfiguration |
| [`meshtastic:///settings/network`](meshtastic:///settings/network) | Netzwerkkonfiguration |
| [`meshtastic:///Einstellungen/Position`](meshtastic:///Einstellungen/Position) | Positionskonfiguration |
| [`meshtastic:///settings/power`](meshtastic:///settings/power) | Energiekonfiguration |
| **Modulkonfiguration** |  |
| [`meshtastic:///settings/ambientLighting`](meshtastic:///settings/ambientLighting) | Umgebungsbeleuchtung |
| [`meshtastic:///settings/cannedMessages`](meshtastic:///settings/cannedMessages) | Einmundbotschaften |
| [`meshtastic:///settings/detectionSensor`](meshtastic:///settings/detectionSensor) | Erkennungssensor |
| [`meshtastic:///settings/externalNotification`](meshtastic:///settings/externalNotification) | Externe Benachrichtigung |
| [`meshtastic:///settings/mqtt`](meshtastic:///settings/mqtt) | MQTT |
| [`meshtastic:///settings/paxCounter`](meshtastic:///settings/paxCounter) | Pax-Zähler |
| [`meshtastic:///settings/rangeTest`](meshtastic:///settings/rangeTest) | Entfernungstest |
| [`meshtastic:///settings/klingelton`](meshtastic:///settings/klingelton) | Klingelton |
| [`meshtastic:///settings/serial`](meshtastic:///settings/serial) | Serie |
| [`meshtastic:///settings/storeAndForward`](meshtastic:///settings/storeAndForward) | Speichern & Weiterleiten |
| [`meshtastic:///settings/telemetry`](meshtastic:///settings/telemetry) | Telemetrie |
| **TAK** |  |
| [`meshtastic:///settings/tak`](meshtastic:///settings/tak) | TAK-Konfiguration |
| **Protokollierung** |  |
| [`meshtastic:///settings/debugLogs`](meshtastic:///settings/debugLogs) | Protokolle Debuggen |
| **Entwickler** |  |
| [`meshtastic:///settings/appFiles`](meshtastic:///settings/appFiles) | App-Dateien |
| [`meshtastic:///Einstellungen/Werkzeuge`](meshtastic:///Einstellungen/Werkzeuge) | Werkzeuge (iOS 18+) |
| [`meshtastic:///settings/coreDataBrowser`](meshtastic:///settings/coreDataBrowser) | Datenbrowser (nur DEBUG) |
| [`meshtastic:///settings/firmwareUpdates`](meshtastic:///settings/firmwareUpdates) | Firmware-Updates |

