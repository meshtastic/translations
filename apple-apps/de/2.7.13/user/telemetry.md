
# Telemetrie & Sensoren

Meshtastic-Knoten können Sensordaten über das Netz hinweg melden, was dir einen Einblick in die physische Umgebung an entfernten Standorten gibt.

## Telemetrietypen

| Art | Daten |
|------|------|
| Gerätemetriken | Batteriestand, Batteriespannung, Kanalauslastung, Sendezeitanteil |
| Umwelt | Temperatur (°C/°F), relative Luftfeuchtigkeit (%), Luftdruck (hPa) |
| Luftqualität | PM1.0, PM2.5, PM10 Partikelzahl (µg/m³) |
| Macht | Spannungs- und Strommessungen von Leistungsüberwachungssensoren |

### Gerätemetriken

| Icon | Staat | Beschreibung |
|------|-------|-------------|
| ![Battery full](../assets/screenshots/batteryFull.png) | Voll | Die Batterie ist gut aufgeladen (≥80 %). |
| ![Battery low](../assets/screenshots/batteryLow.png) | Tiefstand | Die Batterie ist schwach (≤20 %) - lade den Knoten bald auf. |
| ![Battery charging](../assets/screenshots/batteryCharging.png) | Aufladen | Der Knoten ist angeschlossen und vollständig aufgeladen. |
| ![Battery unknown](../assets/screenshots/batteryNil.png) | Unbekannte | Batteriestand wird von diesem Knoten nicht gemeldet. |
| ![Battery plugged in](../assets/screenshots/batteryPluggedIn.png) | Eingesteckt | Der Knoten wird über USB/externe Stromversorgung mit Strom versorgt. |

### Luftqualität

![IAQ Scale](../assets/screenshots/iaqScale.png)

Die Indoor Air Quality Scale zeigt die Kategoriebänder von Excellent (grün) bis Hazardous (kastanienbraun). Die App unterstützt mehrere Anzeigemodi für Luftqualitätsmessungen:

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/aqi_all_modes_dark.png" />
  <img src="../assets/screenshots/aqi_all_modes_light.png" alt="Air Quality Index — all display modes" />
</picture>

### Umwelt

| Icon | Lesen | Beschreibung |
|------|---------|-------------|
| ![Humidity with dew point](../assets/screenshots/humidityWithDew.png) | Luftfeuchtigkeit (mit taupunkt) | Relativer Feuchtigkeitsprozentsatz und berechnete Taupunkttemperatur. |
| ![Humidity without dew point](../assets/screenshots/humidityNoDew.png) | Feuchtigkeit | Nur relativer Feuchtigkeitsanteil. |
| ![Pressure high](../assets/screenshots/pressureHigh.png) | Hochdruck | Luftdruck über dem Normalwert (≥1013 hPa). |
| ![Pressure low](../assets/screenshots/pressureLow.png) | Tiefdruck | Luftdruck unter dem Normalwert (<1013 hPa). |

### Luftstrom

| Gerät | Beschreibung |
|--------|-------------|
| ![Wind full](../assets/screenshots/windFull.png) | Windgeschwindigkeit, Böengeschwindigkeit und Richtung. |
| ![Wind minimal](../assets/screenshots/windMinimal.png) | Nur Windgeschwindigkeit (keine Böen- oder Richtungsdaten verfügbar). |

### Strahlung

| Gerät | Beschreibung |
|--------|-------------|
| ![Radiation](../assets/screenshots/radiation.png) | Strahlungsstärke in µR/Std. von einem angeschlossenen Geiger-Zählersensor. |

## Telemetrie anzeigen

Die Telemetrie ist an zwei Stellen sichtbar:

1. **Knotendetail** - Tippe auf einen beliebigen Knoten auf der Registerkarte "Knoten". Der Abschnitt "Protokolle" zeigt die neuesten Gerätemetriken und Umgebungsmesswerte an.
2. **Telemetriediagramme** - Tippe auf das Diagrammsymbol in einem Knotendetail, um historische Diagramme für jeden Telemetrietyp anzuzeigen, den der Knoten gemeldet hat.

## Telemetrie konfigurieren

Gehe zu **Einstellungen → Telemetrie**, um Telemetriemodule zu aktivieren und Berichtsintervalle festzulegen:

![Telemetry Config](../assets/screenshots/telemetryConfig.png)

| Kulisse | Beschreibung |
|---------|-------------|
| Intervall für Gerätemetriken | Wie oft (Sekunden) der Knoten Batterie- und Auslastungsdaten sendet. |
| Umgebungsintervall | Wie oft Umgebungssensordaten übertragen werden. |
| Luftqualitätsintervall | Wie oft Luftqualitätssensordaten übertragen werden. |
| Umgebungsbildschirm | Umgebungsdaten auf dem Gerätebildschirm anzeigen. |
| Telemetrie im Admin-Kanal | Beschränken du die Telemetrie auf den Admin-Kanal anstelle der Übertragung. |

## Unterstützte Sensoren

Die App zeigt Daten von jedem Sensor an, der von der Meshtastic-Firmware unterstützt wird. Gemeinsame Sensoren:

- **BME280 / BME680** — Temperatur, Luftfeuchtigkeit, Druck
- **SHT31** — Temperatur, Luftfeuchtigkeit
- **MCP9808** — Präzisionstemperatur
- **INA219 / INA260** — Leistungsüberwachung
- **PMSA003** — Luftqualität (PM2.5)

Die Verfügbarkeit des Sensors hängt von deiner Hardware ab. Überprüfe die [Meshtastic Hardware Guide](https://meshtastic.org/docs/hardware/) auf Kompatibilität.

## Erkennungssensor

Das Erkennungssensormodul warnt das Netz, wenn ein angeschlossener PIR-Bewegungssensor oder Kontaktschalter ausgelöst wird. Konfiguriere es in **Einstellungen → Erkennungssensor**. Warnungen werden als Nachrichten auf dem primären Kanal und als Knotenprotokolleinträge angezeigt.

