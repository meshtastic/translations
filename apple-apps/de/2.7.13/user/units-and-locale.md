
# Einheiten, Messung & Gebietsschema

Die Meshtastic-App zeigt automatisch Temperaturen, Entfernungen, Geschwindigkeiten und Zeiten in den Geräten an, für die die Verwendung Ihres Geräts konfiguriert ist - keine Einstellungen, die innerhalb der App geändert werden.

---

## Wie Es Funktioniert

Meshtastic-Funkgeräte übertragen Daten immer in **metrischen Einheiten** (Meter, °C, km/h, hPa usw.). Wenn die App diese Daten erhält, übergibt sie sie an das integrierte Formatierungssystem Ihres Geräts, das Werte in jedem von Ihnen gewählten Einheitensystem unter **Einstellungen → Allgemein → Sprache & Region** konvertiert und anzeigt.

![Language & Region settings](../assets/screenshots/settingsLanguageRegion.png)

Der Bildschirm **Sprache & Region** steuert, wie die Meshtastic-App Temperaturen, Entfernungen, Daten, Zahlen und mehr anzeigt. Schlüsseleinstellungen:

| Kulisse | Was es in Meshtastic steuert |
|---|---|
| **Temperatur** | °C oder °F für alle Sensorwerte und Wetter |
| **Messsystem** | Metrisch (m, km, kg, mm) oder US/UK (ft, mi, lbs, in) |
| **Kalender** | Kalendersystem für alle Daten |
| **Erster Tag der Woche** | Wochentagsbeginn in Datumsanzeigen |
| **Datumsformat** | Datum der Bestellung in der gesamten App |
| **Nummernformat** | Dezimaltrennzeichen und Zifferngruppierung |

> **Tip — Sie müssen niemals Einheiten innerhalb der App umschalten.**Ändern Sie Ihre Systemmesseinstellungen und jeder Bildschirm in Meshtastic wird automatisch aktualisiert - Knotendetails, Telemetriediagramme, Wetter, Höhe und mehr.

## Temperatur

Temperaturwerte von Umgebungssensoren und Wettervorhersagen werden als **°C** übertragen und je nach Temperatureinheit Ihres Geräts entweder als **°C** oder **°F** angezeigt.

| Ihre Einstellung | Sie sehen |
|---|---|
| Celsius | 22 °C |
| Fahrenheit | 72 °F |

Dies betrifft alle Temperaturanzeigen in der App: Knotenumgebungstelemetrie, Bodentemperatur, Taupunkt, Wettervorhersagen und Telemetrie-Diagramm-Achsen.

## Entfernung & Höhe

Entfernungen zwischen Knoten und GPS-Höhen werden als **Meter** übertragen und vom System automatisch skaliert und umgerechnet.

| Ihre Einstellung | Kleine Entfernung | Große Entfernung | Höhe |
|---|---|---|---|
| Metrik | 350 m | 2,5 km | 1.200 m |
| Imperial (USA) | 1.148 Fuß | 1,6 Meilen | 3.937 Fuß |

Die App verwendet natürliche Skalierung - kurze Entfernungen bleiben in Metern oder Fuß, während längere Entfernungen automatisch zu Kilometern oder Meilen wechseln.

### Wo diese erscheinen

- **Knotenliste** - Entfernung und Lager zu jedem Knoten
- **Knotendetail** — Höhe, Entfernung von Ihrer Position
- **Karte** — Wegpunktentfernungen, Spurrouten-Hüfenentfernungen
- **Kompass** — Abstand zum ausgewählten Knoten
- **Höhendiagramm** - Y-Achsen-Beschriftungen passen sich Ihrem Gebietsschema an

## Geschwindigkeit

Die GPS-Bodengeschwindigkeit wird in der bevorzugten Geschwindigkeitseinheit Ihres Standorts angezeigt.

| Ihre Einstellung | Sie sehen |
|---|---|
| Metrik | 12 km/h |
| Imperial (USA) | 7 Meilen pro Stunde |

Die Geschwindigkeit wird auf dem Bildschirm **GPS-Status** angezeigt, wenn Ihr Gerät über einen aktiven GPS-Fix verfügt.

## Luftstrom

Windgeschwindigkeits- und Böendaten von Umgebungssensoren werden als **m/s** übertragen und für die Anzeige umgerechnet.

| Ihre Einstellung | Sie sehen |
|---|---|
| Metrik | 5 m/s |
| Imperial (USA) | 11 Meilen pro Stunde |

Windmesswerte erscheinen im Abschnitt **Knotendetail** Wetter und in den **Umgebungstelemetrie**-Protokollspalten.

## Gewicht

Die Gewichtstelemetrie wird als **kg** übertragen und für die Anzeige umgewandelt.

| Ihre Einstellung | Sie sehen |
|---|---|
| Metrik | 24,5 kg |
| Imperial (USA) | 54,0 Pfund |

## Niederschlag

Niederschlagsmessungen (Gesamt 1 Stunde und 24 Stunden) werden als **mm** übertragen und für die Anzeige umgerechnet.

| Ihre Einstellung | Sie sehen |
|---|---|
| Metrik | 12 mm |
| Imperial (USA) | 0,5 in |

## Einheiten, die sich nie ändern

Einige Einheiten sind internationale Standards und werden unabhängig von Ihrem Gebietsschema auf die gleiche Weise angezeigt:

| Messung | Einheit | Warum |
|---|---|---|
| Luftdruck | HPa | Internationale meteorologische Norm |
| Überschrift / Lager | ° (Grad) | Universelle Navigationskonvention |
| Strahlung | µR/Std. | Standard-Dosimetrieeinheit |
| GPS-Koordinaten | Dezimalgrad | Universeller geografischer Standard |
| Luftfeuchtigkeit, Batterie, Bodenfeuchtigkeit | Prozent | Universell |

## Datum & Uhrzeit

Alle Zeitstempel in der gesamten App - zuletzt gehört, Nachrichtenzeiten, Telemetrieprotokolle, Diagrammachsen - folgen den Datums- und Uhrzeiteinstellungen Ihres Geräts.

| Kulisse | Was Es Steuert | Beispiel |
|---|---|---|
| **24-Stunden-Zeit** | Taktformat | 14:30 vs 14:30 Uhr |
| **Datumsformat** | Datum der Bestellung | 09/05/2026 vs 05/09/2026 vs 2026-05-09 |
| **Kalender** | Kalendersystem | Gregorianisch, buddhistisch, japanisch usw. |

Die App verwendet auch **relative Zeit**, wenn es sinnvoll ist - zum Beispiel "vor 5 Minuten" oder "vor 2 Stunden" in der Knotenliste - die automatisch in der Sprache Ihres Geräts lokalisiert wird.

## Ändern Sie Ihr Messsystem

Ihr Messsystem (metrisch vs. imperial) ist an Ihre Regionseinstellung gebunden. Um es zu ändern, ohne Ihre Sprache zu ändern:

1. Öffnen **Einstellungen → Allgemein → Sprache & Region**
2. Tippen Sie auf **Messsystem**
3. Wählen Sie **Metrisch**, **US** oder **UK**

Die Meshtastic-App nimmt die Änderung sofort auf - kein Neustart erforderlich.

> **Tip — UK gegen US Imperial.**Das britische Messsystem verwendet Meilen für die Entfernung, aber Steine für das Körpergewicht und Celsius für die Temperatur. Das US-System verwendet Fahrenheit und Pfund. Die App respektiert diese Unterscheidungen automatisch.

