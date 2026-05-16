
# Apple Watch App

Die Meshtastic Apple Watch-App ist ein Begleiter zur iPhone-App, die zwei Funktionen auf dein Handgelenk setzt: einen **Foxhunt-Kompass** zur Funkrichtungsfindung und ein **Telefon-Verbindungspanel**, um zu bestätigen, dass deine Uhr synchronisiert ist.

Knotendaten werden automatisch an die Watch gesendet, wenn sich die iPhone-App über WatchConnectivity in Reichweite befindet. Auf der Watch selbst ist keine Bluetooth-Verbindung zu deinem Meshtastic-Radio erforderlich.

## Anforderungen

| Anforderung | Details |
|-------------|---------|
| Apple Watch | watchOS mit iPhone gekoppelt |
| iPhone-App | Meshtastic iPhone App geöffnet und mit einem Radio verbunden |
| Ort | Watch Location Services zur Orientierungsfindung aktiviert |
| Nähe | Uhr und iPhone im normalen Bluetooth/Wi-Fi-Bereich voneinander |

## Registerkarten

Die Watch-App verwendet ein vertikales Seitenlayout. Wischen du nach oben oder unten, um zwischen den beiden Tabs zu wechseln.

### Fuchsjagd

Die Registerkarte Foxhunt listet Mesh-Knoten auf, die sich innerhalb von **½ Meile (≈ 800 m)** von deinem aktuellen Watch-Standort befinden und eine bekannte GPS-Position haben. Knoten, die von der iPhone-App als Foxhunt-Ziele gekennzeichnet sind, erscheinen immer ganz oben in der Liste, unabhängig von der Entfernung.

Jede Zeile zeigt:

| Element | Bedeutung |
|---------|---------|
| Farbiger Kreis | Knoten-Kurzname, Farbe abgeleitet von Knotennummer |
| Name | Knoten langer Name |
| Entfernung | Entfernung zu deinem aktuellen Standort, farblich gekennzeichnet durch Nähe |
| Pfeil | Mini-Pfeil, der auf den Knoten zeigt, dreht sich mit deiner Überschrift |

Tippen du auf eine beliebige Zeile, um den **Foxhunt Compass** für diesen Knoten zu öffnen.

#### Fuchsjagd Kompass

Der Kompass zeigt mit dem Richtungssensor deiner Uhr auf den ausgewählten Knoten. Es ist für die Funkrichtungssuche (Foxhunting) konzipiert - gehen du, bis der Pfeil geradeaus zeigt und die Entfernung Null anzeigt.

| Element | Bedeutung |
|---------|---------|
| Drehbares Zifferblatt | Kardinalrichtungen (N/NE/E...) drehen du sich mit deiner physischen Richtung |
| Orangefarbenes Dreieck | Feste Nordanzeige oben im Ring |
| Farbiger Pfeil | Pfeillager, der auf den Zielknoten zeigt |
| Richtungskegel | Transluzenter Keil, der die Zielrichtung hervorhebt |
| Mittelkreis | Aktuelle Richtung in Grad, Richtung zum Ziel und Entfernung |
| Knotenkreis | Kurzes Namensschild des Zielknotens |

**Entfernungsfarbcodierung:**

| Farbe | Entfernung |
|--------|----------|
| Rot | Weit (> 2⁄3 von ½ Meile) |
| Gelb | Mittelklasse (1⁄3 – 2⁄3 von ½ Meile) |
| Grün | Schließen (< 1⁄3 von ½ Meile) |

**Haptisches Feedback:** Die Watch tippt auf dein Handgelenk, wenn du sich innerhalb von 10° um das Lager des Zielknotens befinden – nützlich, wenn du nicht auf den Bildschirm schauen können.

### Telefon

Auf der Registerkarte „Telefon“ wird der Verbindungsstatus zwischen deiner Watch und der zugehörigen iPhone-App angezeigt.

| Staat | Bedeutung |
|-------|---------|
| Telefon verbunden (grün) | iPhone-App ist erreichbar; Anzahl der Knoten angezeigt |
| Telefon nicht erreichbar | Die Uhr ist außerhalb der Reichweite oder die iPhone-App läuft nicht |

Tippen du auf **Aktualisieren**, um eine aktualisierte Knotenliste aus der iPhone-App anzufordern. Wenn das Telefon vorübergehend nicht erreichbar ist, greift die Watch auf die zuletzt empfangenen Knotendaten zurück.

## Foxhunt-Ziele festlegen

Markieren du in der iPhone-App einen Knoten als Foxhunt-Ziel aus der Detailansicht. Markierte Knoten werden auf die Uhr geschoben und unabhängig von der Entfernung an den oberen Rand der Foxhunt-Liste angeheftet - nützlich, wenn du wissen, welchen Knoten du jagen, bevor du sich in einer Entfernung von ½ Meile befinden.

