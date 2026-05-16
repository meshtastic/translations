
# Einstellungen
Auf der Registerkarte "Einstellungen" können Sie die App und Ihr angeschlossenes Meshtastic-Funkgerät konfigurieren.
## App-Einstellungen
Allgemeine App-Einstellungen, einschließlich Kartenstil, Benachrichtigungsverhalten und Thema. Diese betreffen nur die App - nicht das Radio.
## Funkkonfiguration
Die Funkkonfiguration erfordert einen angeschlossenen Knoten. Wählen Sie Ihren Knoten aus dem Abschnitt **Konfigurieren**, wenn Sie mehrere Knoten haben.
### LoRa
Die LoRa-Einstellungen steuern, wie Ihr Radio auf dem Netz kommuniziert:
| Kulisse | Beschreibung |
|---------|-------------|
| Region | Ihre geografische Region. **Muss korrekt eingestellt sein** - die Verwendung der falschen Region ist illegal und verhindert die Kommunikation mit lokalen Knoten. Alle im Meshtastic-Protobuf definierten Regionen sind verfügbar, einschließlich Nepal 865MHz, Brasilien 902MHz, ITU Region 1 Amateur 2m, ITU Region 2 & 3 Amateur 2m, EU 866, EU 874, EU 917 und das weltweite 2,4-GHz-Band. |
| Modemvoreinstellung | Kompromiss zwischen Geschwindigkeit und Reichweite. Die meisten Benutzer sollten Long Fast oder Long Slow verwenden. |
| Hop-Limit | Die Anzahl der Wiederholungen einer Nachricht durch andere Knoten. Höhere Werte erhöhen die Reichweite, aber auch den Datenverkehr. |
| Frequenz-Slot | Feinabstimmung der genauen Frequenz in Ihrer Region. |

### Kanäle
Verwalten Sie bis zu 8 Kanäle (0–7). Kanal 0 ist der primäre Rundfunkkanal. Zusätzliche Kanäle erstellen isolierte Messaging-Gruppen mit ihren eigenen Verschlüsselungsschlüsseln.
### Sicherheit
Konfigurieren Sie die PKI-Verschlüsselung (Public Key Infrastructure) für Direktnachrichten. Erfordert Firmware 2.5+.
### Benutzer
Legen Sie Ihren Long Name (Anzeigenamen) und Short Name (4 Zeichen/Emoji-Kennung im Knotenkreis) fest.
### Bluetooth
BLE-Funkeinstellungen einschließlich PIN-Modus und Energieeinsparung. Änderungen gelten beim nächsten Radio-Neustart.
### Gerät
Geräterolle, serielle Ausgabe, Debug-Protokoll-Streaming und Knoten-Info-Übertragungsintervall.
### Anzeige
Bildschirm-Timeout, automatisches Karussell der Bildschirme, Flip-Screen für alternative Montageausrichtungen und OLED-Kontrast.
### Netzwerk
Wi-Fi SSID/Passwort für TCP-Verbindung, NTP-Server und Ethernet (nur unterstützte Hardware).
### Position
GPS-Update-Intervall, Positionsgenauigkeit und intelligente Positionsübertragung. Aktivieren Sie **Broadcast-Position**, um Ihren Standort mit dem Netz zu teilen.
### Macht
Batteriesparprofile, Schlafmodi und minimale Weckzeit. Entscheidend für solarbetriebene Routerknoten.
## Modulkonfiguration
Optionale Funktionsmodule. Nur verfügbar, wenn Ihr angeschlossener Knoten das Modul unterstützt.
| Modul | Beschreibung |
|--------|-------------|
| Umgebungsbeleuchtung | Steuern Sie die NeoPixel/LED-Beleuchtung auf unterstützter Hardware. |
| Einmundbotschaften | Vorprogrammierte Nachrichtenverknüpfungen, die über die Gerätetasten zugänglich sind. |
| Erkennungssensor | PIR-Bewegungs- oder Kontaktsensoren konfigurieren. |
| Externe Benachrichtigung | Summer- oder LED-Warnungen für eingehende Nachrichten. |
| MQTT | Uplink-/Downlink-Nachrichten an einen MQTT-Broker für Internet-Brücken. |
| Entfernungstest | Automatisierte Reichweitenprüfung mit Positionsprotokollierung. |
| Pax-Zähler | Anonymisierte Zählung des Fußgängerverkehrs über Bluetooth/Wi-Fi-Sondenerkennung. |
| Klingelton | Benutzerdefinierte RTTTL-Melodien für Benachrichtigungstöne. |
| Speichern & Weiterleiten | Speichern Sie Pakete für Knoten, die vorübergehend offline sind. |
| Serie | UART-Serielle Ausgabe für die Integration mit anderer Hardware. |
| Telemetrie | Berichterstattung über Geräte, Umgebungs- und Luftqualitätssensoren. |

## Firmware-Updates
Suchen und wenden Sie OTA-Firmware-Updates direkt über die App an Ihrem angeschlossenen Radio an. Siehe [Firmware-Updates](firmware.md) für alle Details.
## Automatische Dokumentationsübersetzung
Auf Geräten mit iOS 26 oder höher wird die In-App-Dokumentation automatisch in die Sprache Ihres Geräts übersetzt, wenn sie sich vom Englischen unterscheidet.
### Wie Es Funktioniert
- **Spracherkennung**: Die App liest jedes Mal, wenn Sie eine Dokumentationsseite öffnen, die primäre Spracheinstellung Ihres Geräts.- **Übersetzung auf dem Gerät**: Seiten werden mit dem Übersetzungs-Framework von Apple (iOS 26+) von Apple übersetzt. Wenn eine Sprache vom Übersetzungs-Framework nicht unterstützt wird, greift die App auf das Foundation-Modell auf dem Gerät zurück (nur iOS 26+).- **Kein Netzwerk erforderlich**: Nach der ersten Übersetzung sind alle Inhalte offline verfügbar.- **Caching**: Übersetzte Seiten werden lokal gespeichert, so dass sie bei nachfolgenden Besuchen sofort geladen werden.- **Hintergrund-Prefetch**: Nachdem die aktuelle Seite übersetzt wurde, werden die restlichen Seiten im Hintergrund mit niedriger Priorität übersetzt.
### Fallback auf Englisch
Wenn die Übersetzung nicht verfügbar ist (älter als iOS 26, nicht unterstützte Sprache oder Sprachpaket nicht heruntergeladen), wird die Originaldokumentation in Englisch angezeigt. Die App zeigt nie leere oder defekte Seiten an.
### Cache-Verwaltung
- Übersetzte Dateien werden in der Anwendungsunterstützung gespeichert und bleiben bei App-Starts bestehen.- Ein Limit von 50 MB pro Sprache wird mit der am wenigsten zuletzt verwendeten Räumung durchgesetzt.- Wenn die englische Quelldokumentation aktualisiert wird (neue App-Version), werden abgestangene Übersetzungen automatisch generiert.
> **Tip — Sprachänderung**: Wenn Sie die Sprache Ihres Geräts ändern, während die App geöffnet ist, werden die Dokumentationsseiten automatisch in der neuen Sprache neu geladen.
