
# Übersetze die App
Beiträge zu Übersetzungen zur Meshtastic Apple App hilft, das Projekt für ein breiteres Publikum zugänglich zu machen. Die App verwendet [String-Kataloge](https://developer.apple.com/documentation/xcode/localizing-and-varying-text-with-a-string-catalog) in Xcode, um Übersetzungen zu verwalten.
## Automatische Dokumentationsübersetzung
Auf Geräten mit iOS 26 oder höher wird die In-App-Dokumentation automatisch in die Sprache Ihres Geräts übersetzt, wenn Sie den Abschnitt **Hilfe & Dokumente** öffnen. Die Übersetzungspipeline funktioniert wie folgt:
1. Die App liest die gebündelten englischen Markdown-Quelldateien.2. Textsegmente werden mit dem Übersetzungs-Framework von Apple übersetzt. Wenn das Übersetzungs-Framework Ihre Sprache nicht unterstützt, greift die App auf FoundationModels auf dem Gerät zurück.3. Der übersetzte Markdown wird lokal zwischengespeichert, so dass nachfolgende Besuche sofort geladen werden.4. Der übersetzte Markdown wird auf dem Gerät in HTML konvertiert und im Docs Viewer angezeigt.
Nachdem alle Dokumentationsseiten im Hintergrund übersetzt wurden, lädt die App die übersetzten Markdown-Dateien automatisch in das [meshtastic/translations](https://github.com/meshtastic/translations)-Repository hoch. Dies ermöglicht es der Community, maschinell generierte Übersetzungen zu überprüfen und zu verbessern.
> **Tip — Englische Benutzer** Wenn die Sprache Ihres Geräts Englisch ist, findet keine Übersetzung statt und die gebündelte englische Dokumentation wird direkt angezeigt.
## Wie man UI-Übersetzungen beisteuert
Wenn Sie die Übersetzungen für ein bestehendes Gebietsschema aktualisieren oder eine neue Sprache hinzufügen möchten, gehen Sie wie folgt vor:
1. Gabeln Sie das [Meshtastic-Apple-Repository](https://github.com/meshtastic/Meshtastic-Apple/tree/main) in Ihr GitHub-Konto.2. Klonen Sie das Projekt und öffnen Sie`Meshtastic.xcworkspace`In Xcode.3. Wählen Sie die`Localizable.xcstrings`Datei im Projektnavigator.4. Befolgen Sie die [Schritte zum Hinzufügen oder Aktualisieren von Übersetzungen](https://developer.apple.com/documentation/xcode/localizing-and-varying-text-with-a-string-catalog) in der Apple-Dokumentation.5. Erstellen Sie einen Pull-Request für das Projekt mit Ihren Änderungen.
Ihr Beitrag wird überprüft und nach Genehmigung wird Ihre Übersetzung in die nächste Version der Meshtastic Apple App aufgenommen.
> **Tip — Neue Sprache? ** Wenn Sie eine Sprache hinzufügen, die noch nicht im Projekt vorhanden ist, öffnen Sie die Xcode-Projekteinstellungen, gehen Sie zu **Info → Lokalisierungen** und fügen Sie vor der Bearbeitung das neue Gebietsschema hinzu`Localizable.xcstrings`.
Vielen Dank, dass Sie dazu beigetragen haben, die Reichweite von Meshtastic zu erweitern!
