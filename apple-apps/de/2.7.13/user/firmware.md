
# Firmware-Updates

Die App kann über Bluetooth nach Meshtastic-Firmware-Updates suchen und direkt auf Ihrem angeschlossenen Radio installieren.

## Nach Updates prüfen

1. Verbinden Sie sich mit Ihrem Radio.
2. Gehen Sie zu **Einstellungen → Firmware-Updates**.
3. Die App zeigt die Firmware-Version, die derzeit auf Ihrem Radio läuft, und die neueste stabile Version, die von GitHub verfügbar ist.

## Installieren eines Updates

1. Tippen Sie auf **Firmware aktualisieren**, wenn eine neuere Version verfügbar ist.
2. Die App lädt die korrekte Firmware-Binärdatei für Ihre Hardware herunter.
3. Das Radio wechselt in den Aktualisierungsmodus (DFU) und die neue Firmware wird über BLE übertragen.
4. Das Radio startet automatisch neu, wenn das Update abgeschlossen ist.

| Icon | Fortschritt | Beschreibung |
|------|----------|-------------|
| ![0%](../assets/screenshots/progressZero.png) | Beginnend | Initiieren des Updates - Firmware-Binär-Download. |
| ![50%](../assets/screenshots/progressHalf.png) | In Bearbeitung | Firmware-Übertragung im Gange über BLE. |
| ![Complete](../assets/screenshots/progressComplete.png) | Komplett | Übertragung abgeschlossen - Radio startet neu. |
| ![Error](../assets/screenshots/progressError.png) | Fehler | Update fehlgeschlagen – siehe Fehlerbehebung unten. |

**Schren Sie die App nicht und verlassen Sie die Bluetooth-Reichweite während eines Firmware-Updates nicht. **

## Kanäle aktualisieren

| Kanal | Beschreibung |
|---------|-------------|
| Stabil | Empfohlen für die meisten Benutzer. Getestete Veröffentlichungen. |
| Alpha | Früher Zugriff - kann Fehler enthalten. Nur auf Sekundär-/Testgeräten verwenden. |

Wählen Sie den Aktualisierungskanal unter **Einstellungen → App-Einstellungen → Firmware-Kanal**.

## Vermittlung

**Update schlägt auf halbem Weg fehl**
- Halten Sie das Radio während des Updates innerhalb von 1–2 Metern von Ihrem Telefon.
- Wenn das Radio nach einem fehlgeschlagenen Update gemauert erscheint, kann es normalerweise mit dem [Meshtastic Flasher](https://flasher.meshtastic.org/) auf einem Computer wiederhergestellt werden.

![Incompatible firmware version warning](../assets/screenshots/invalidVersion.png)

![Security update recommended](../assets/screenshots/securityVersionNag.png)

**Radio wird nicht in der Firmware-Liste angezeigt**
- Die Firmware-Update-Funktion erfordert ein angeschlossenes Funkgerät (BLE oder TCP).
- Einige ältere Radios unterstützen keine OTA-Updates. Überprüfen Sie die [Hardware-Kompatibilitätsliste](https://meshtastic.org/docs/hardware/).

**Version wird als unbekannt angezeigt**
- Stellen Sie sicher, dass das Radio vollständig angeschlossen und synchronisiert ist (es dauert normalerweise 5-10 Sekunden, nachdem die BLE-Verbindung hergestellt wurde).

