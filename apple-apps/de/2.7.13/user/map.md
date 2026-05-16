
# Karte & Wegpunkte

Die Registerkarte "Karte" zeigt alle Knoten an, die eine Position geteilt haben, die auf einer Apple Maps-Basisschicht überlagert sind.

## Knotenstifte

Jeder Knoten, der eine GPS-Position gemeldet hat, erscheint als farbige Kreisnadel auf der Karte. Die **grüne durchgehende Linie** zeigt einen direkt verbundenen Knoten; **orange gestrichene Linien** zeigen Knoten, die über das Netz erreicht werden. Ein lila Stern markiert einen Wegpunkt. Tippen Sie auf eine Stecknadel, um den Knotennamen, die zuletzt gehörte Uhrzeit, die Signalinformationen und eine Verknüpfung zum Senden einer Direktnachricht anzuzeigen.

Pins werden automatisch aktualisiert, wenn ein neues Positionspaket vom Netz empfangen wird.

## Kartenebenen

Tippen Sie auf das Ebenensymbol (oben rechts), um zwischen:

| Schicht | Beschreibung |
|-------|-------------|
| Prüfstein | Standardmäßig Apple Maps Straßen-/Satelliten-Hybrid |
| Satellit | Luftbilder |
| GeoJSON-Überlagerungen | Benutzerdefinierte Kartenebenen geladen von`.geojson`Dateien im Dateispeicher der App |

## Wegpunkte

Wegpunkte werden als Points of Interest bezeichnet, die Sie im gesamten Netz teilen können.

### Erstellen eines Wegpunkts

1. Drücken Sie lange auf eine beliebige Stelle auf der Karte.
2. Geben Sie einen Namen, eine optionale Beschreibung und ein Schlosssymbol ein (um die Bearbeitung auf den Ersteller zu beschränken).
3. Tippen Sie auf **Speichern** – der Wegpunkt sendet an alle Knoten auf dem primären Kanal.

### Bearbeiten eines Wegpunkts

Tippen Sie auf einen vorhandenen Wegpunktstift und dann auf **Bearbeiten**. Ändert die Übertragung auf das Netz sofort.

### Löschen eines Wegpunkts

Tippen Sie auf den Wegpunkt und dann auf **Löschen**. Die Löschung sendet an alle Knoten.

## Knotenpfad

Wenn ein Knoten im Laufe der Zeit mehrere Positionen gemeldet hat, verbindet eine Spurlinie die historischen Positionen auf der Karte und zeigt den Pfad des Knotens an.

## Ihr Standort

Ihre aktuelle GPS-Position wird als blauer Punkt (Standard-iOS-Standortanzeige) angezeigt. Aktivieren Sie die Positionsübertragung in **Einstellungen → Position**, um Ihren Standort mit dem Netz zu teilen.

