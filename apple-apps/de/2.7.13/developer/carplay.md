
# CarPlay-Architektur

Diese Seite behandelt die Implementierung der CarPlay-Funktion. Die benutzerorientierte Anleitung finden Sie unter [CarPlay](../user/carplay.md).

## Komponenten

| Komponente | Dossier | Beschreibung |
|---|---|---|
| `CarPlaySceneDelegate` | `Meshtastic/CarPlay/CarPlaySceneDelegate.swift` | `CPTemplateApplicationSceneDelegate`Die die Benutzeroberfläche mit zwei Registerkarten erstellt und verwaltet |
| `CarPlayIntentDonation` | `Meshtastic/CarPlay/CarPlayIntentDonation.swift` | Spendet eingehende und ausgehende`INSendMessageIntent`Interaktionen, damit Gespräche in CarPlay-Nachrichten angezeigt werden und Siri sie laut vorlesen kann |
| `SendMessageIntentHandler` | `Meshtastic/Intents/SendMessageIntentHandler.swift` | Griffe`INSendMessageIntent`— Löst Empfänger/Kanäle auf und sendet die Nachricht über den aktiven Transport |
| `SearchForMessagesIntentHandler` | `Meshtastic/Intents/SearchForMessagesIntentHandler.swift` | Griffe`INSearchForMessagesIntent` |
| `SetMessageAttributeIntentHandler` | `Meshtastic/Intents/SetMessageAttributeIntentHandler.swift` | Griffe`INSetMessageAttributeIntent`(Als gelesen markieren) |
| `IntentHandler` | `Meshtastic/Intents/IntentHandler.swift` | Routen`INIntent`S an den entsprechenden Handler |

## Vorlagenaktualisierungen

Der Szenendelegierte abonniert`AccessoryManager.shared.$isConnected`Mit einem **300 ms Debounce** und Anrufen`updateSections(_:)`Auf bestehenden`CPListTemplate`Instanzen, anstatt den gesamten Vorlagenbaum neu zu erstellen. Dies minimiert das Flimmern beim erneuten Verbinden und vermeidet das Auslösen der Geschwindigkeitsbegrenzung von CarPlay beim Vorlagenaustausch.

## De-Duplizierung der Absichtsspende

Absichtsspenden werden pro CarPlay-Sitzung mit einem In-Memory-`Set`. Dies vermeidet wiederholte IPC-Aufrufe an den Intents-Daemon bei jeder Listenaktualisierung (was bei einem Timer auftritt, während CarPlay verbunden ist).

Wenn eine neue CarPlay-Sitzung beginnt, wird das Set gelöscht und bis zu 50 ungelesene Nachrichten werden im Stapel gespendet, damit Siri sie bei Bedarf zurücklesen kann.

## Hinzufügen einer neuen Absicht

1. Erstellen Sie einen Handler in`Meshtastic/Intents/`Anpassung an die entsprechenden`INIntent`Protokoll.
2. Registrieren Sie den Handler in`IntentHandler.swift`'S`handler(for:)`Schalter.
3. Absichtserklärung in`Meshtastic.entitlements`Unter`com.apple.developer.siri`.
4. Fügen Sie eine Nutzungsbeschreibung in`Info.plist`Wenn die Absicht eine neue Datenschutzberechtigung erfordert.

