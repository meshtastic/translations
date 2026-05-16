
# CarPlay

Die App unterstützt Apple CarPlay für **freihändige Mesh-Nachrichten** während der Fahrt. Die CarPlay-Oberfläche lässt sich in das iOS-Nachrichtensystem und Siri integrieren, sodass Benutzer Meshtastic-Nachrichten senden und empfangen können, ohne auf ihr Telefon zu schauen.

## Anforderungen

- IPhone mit iOS 17.5 oder neuer
- Eine unterstützte CarPlay-Kopfeinheit oder der CarPlay-Simulator in Xcode
- Ein Meshtastic-Gerät, das über Bluetooth, TCP oder seriell verbunden ist
- Siri aktiviert - die App fordert Siri-Autorisierung während des Onboardings und erneut bei nachfolgenden Starts an

## Benutzeroberfläche

Der CarPlay-Bildschirm zeigt eine **Schnittstelle mit zwei Registerkarten**:

| Lasche | Beschreibung |
|-----|-------------|
| **Kanäle** | Listet alle aktiven Mesh-Kanäle auf |
| **Direktnachrichten** | Listet aktuelle und bevorzugte Kontakte auf |

Wenn kein Meshtastic-Gerät verbunden ist, zeigen beide Registerkarten ein **"Nicht verbunden"** Statuselement mit einer Aufforderung, die Meshtastic-App zu öffnen.

### Registerkarte Kanäle

Jede Kanalreihe zeigt:
- Der Kanalname (oder "Primary Channel" für Index 0)
- Ein ungelesenes Nachrichtenabzeichen, wenn es ungelesene Nachrichten gibt
- "Primär" oder "Ch N" als Detailtext

Das Tippen auf eine Kanalzeile startet eine Siri-Komponiersitzung für diesen Kanal.

### Registerkarte "Direktnachrichten"

Der Tab "Direktnachrichten" ist in zwei Abschnitte unterteilt:

- **Favoriten** — Als Favoriten markierte Knoten, sortiert nach zuletzt gehört
- **Zuletzt** - Alle anderen nachrichtenfähigen Kontakte mit Verlauf, sortiert nach zuletzt gehört (bezerkenkt auf 24 Einträge)

Jede Kontaktzeile zeigt:
- Kontaktname und ein Personensymbol
- Anzahl der ungelesenen Nachrichten, falls zutreffend
- Zeit seit dem letzten Hören (z.B. "Gerade jetzt", "vor 5 Minuten", "vor 2 Stunden", "vor 3 Tagen")

## Siri Sprachbefehle

Verwenden Sie diese Siri-Sprachbefehle in CarPlay, um mit Meshtastic zu interagieren:

| Sprachbefehl | Beispielsatz | Beschreibung |
|---|---|---|
| Nachricht senden | "Senden Sie eine Nachricht auf Meshtastic" | Verfasst und sendet eine Textnachricht an einen Kontakt oder Kanal |
| Nachrichten suchen | "Suche nach Meshtastic-Nachrichten" | Durchsucht den Nachrichtenverlauf |
| Alesen markieren | "Meshtastic-Nachricht als gelesen markieren" | Markiert eine Unterhaltung als gelesen |

> **Warning — Nachrichtenlimits:**
> Nachrichten sind auf **200 Bytes** (UTF-8) begrenzt. Siri sendet keine Nachrichten, die dieses Limit überschreiten. Pro Nachricht wird nur ein **Einzelempfänger** unterstützt – keine Gruppen-Direktnachrichten. Nur Emoji-Nachrichten und Admin-Nachrichten sind von CarPlay ausgeschlossen.

## Eingehende Nachrichtenankündigungen

Wenn CarPlay verbunden ist und **Benachrichtigungen ankündigen** in iOS-Einstellungen aktiviert ist → Siri, liest Siri eingehende Meshtastic-Nachrichten laut vor. Nur Nicht-Emoji-, Nicht-Admin-Textnachrichten lösen Ankündigungen aus.

Bis zu 50 ungelesene Nachrichten, die vor Beginn der CarPlay-Sitzung eintreffen, werden zum Zeitpunkt der Verbindung an Siri gespendet, damit sie bei Bedarf wieder gelesen werden können.

## Live-Aktivität

Wenn ein Meshtastic-Gerät während einer CarPlay-Sitzung eine Verbindung herstellt, wird automatisch eine **Dynamic Island / Lock Screen Live Activity** gestartet (nur iOS, nicht verfügbar unter macOS). Es zeigt:

- Knotenname und Kurzname
- Betriebszeit, Kanalauslastung und TX-Prozentsatz der Sendezeit
- Pakete gesendet, empfangen und weitergeleitete Statistiken
- Online- und Gesamtzahl der Knoten
- Ein 15-minütiger Countdown-Timer synchronisiert mit dem Telemetrie-Berichtsintervall

Die Live-Aktivität endet automatisch, wenn CarPlay die Verbindung getrennt wird.

> **Tip —** Einzelheiten zur Implementierung und zur Komponentenarchitektur finden Sie im [CarPlay-Entwicklerhandbuch](../developer/carplay.md).


