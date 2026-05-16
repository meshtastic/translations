
# Firmware-Updates

Die App kann über Bluetooth nach Meshtastic-Firmware-Updates suchen und direkt auf deinem angeschlossenen Radio installieren.

## Nach Updates prüfen

1. Verbinde sich mit deinem Radio.
2. Gehe zu **Einstellungen → Firmware-Updates**.
3. Die App zeigt die Firmware-Version, die derzeit auf deinem Radio läuft, und die neueste stabile Version, die von GitHub verfügbar ist.

## Installieren eines Updates

1. Tippe auf **Firmware aktualisieren**, wenn eine neuere Version verfügbar ist.
2. Die App lädt die korrekte Firmware-Binärdatei für deine Hardware herunter.
3. Das Radio wechselt in den Aktualisierungsmodus (DFU) und die neue Firmware wird über BLE übertragen.
4. Das Radio startet automatisch neu, wenn das Update abgeschlossen ist.

| Icon | Fortschritt | Beschreibung |
|------|----------|-------------|
| ![0%](../assets/screenshots/progressZero.png) | Beginnend | Initiieren des Updates - Firmware-Binär-Download. |
| ![50%](../assets/screenshots/progressHalf.png) | In Bearbeitung | Firmware-Übertragung im Gange über BLE. |
| ![Complete](../assets/screenshots/progressComplete.png) | Komplett | Übertragung abgeschlossen - Radio startet neu. |
| ![Error](../assets/screenshots/progressError.png) | Fehler | Update fehlgeschlagen – siehe Fehlerbehebung unten. |

**Schren du die App nicht und verlassen du die Bluetooth-Reichweite während eines Firmware-Updates nicht. **

## Kanäle aktualisieren

| Kanal | Beschreibung |
|---------|-------------|
| Stabil | Empfohlen für die meisten Benutzer. Getestete Veröffentlichungen. |
| Alpha | Früher Zugriff - kann Fehler enthalten. Nur auf Sekundär-/Testgeräten verwenden. |

Wähle den Aktualisierungskanal unter **Einstellungen → Firmware-Updates** aus.

## Vermittlung

**Update schlägt auf halbem Weg fehl**
- Halte das Radio während des Updates innerhalb von 1–2 Metern von deinem Telefon.
- Wenn das Radio nach einem fehlgeschlagenen Update gemauert erscheint, kann es normalerweise mit dem [Meshtastic Flasher](https://flasher.meshtastic.org/) auf einem Computer wiederhergestellt werden.

![Incompatible firmware version warning](../assets/screenshots/invalidVersion.png)

![Security update recommended](../assets/screenshots/securityVersionNag.png)

**Radio wird nicht in der Firmware-Liste angezeigt**
- Die Firmware-Update-Funktion erfordert ein angeschlossenes Funkgerät (BLE oder TCP).
- Einige ältere Radios unterstützen keine OTA-Updates. Überprüfe die [Hardware-Kompatibilitätsliste](https://meshtastic.org/docs/hardware/).

**Version wird als unbekannt angezeigt**
- Stelle sicher, dass das Radio vollständig angeschlossen und synchronisiert ist (es dauert normalerweise 5-10 Sekunden, nachdem die BLE-Verbindung hergestellt wurde).

