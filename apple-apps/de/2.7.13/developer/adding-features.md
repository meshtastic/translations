
# Hinzufügen von Funktionen
Dieser Leitfaden führt Sie durch das Hinzufügen einer neuen Einstellungsansicht von Ende zu Ende: Navigationsstatus, Deep Link, Ansichtsdatei und Integration von Einstellungen.
## 1. Navigationsgehäuse hinzufügen
Offen`Meshtastic/Router/NavigationState.swift`Und fügen Sie einen neuen Fall zu`SettingsNavigationState`:
```swift
enum SettingsNavigationState: String {
    // ... existing cases ...
    case myNewFeature   // raw value is "myNewFeature" — matches the deep link path segment
}
```

Verwenden`lowerCamelCase`Rohwerte. Der Rohwert wird zum URL-Pfadsegment für Deep Linking.
## 2. Erstellen Sie die Ansicht
Erstellen Sie eine neue SwiftUI-Datei unter`Meshtastic/Views/Settings/`:
```swift
// Meshtastic/Views/Settings/MyNewFeatureView.swift

import SwiftUI

struct MyNewFeatureView: View {
    var body: some View {
        List {
            // content
        }
        .navigationTitle("My New Feature")
    }
}
```

Halten Sie die Ansichtsdateien fokussiert. Wenn die Ansicht über ~400 Zeilen hinausgeht (SwiftLint-Warnung), teilen Sie sie in Unteransichten oder Erweiterungsdateien auf.
## 3. Navigation "Einstellungen" verdrahten
Offen`Meshtastic/Views/Settings/Settings.swift`Und finde die`navigationDestination(for:)`Schalter. Fügen Sie Ihren Fall hinzu:
```swift
.navigationDestination(for: SettingsNavigationState.self) { state in
    switch state {
    // ... existing cases ...
    case .myNewFeature:
        MyNewFeatureView()
    }
}
```

Fügen Sie dann eine`NavigationLink`Im entsprechenden Abschnitt der Einstellungsliste:
```swift
NavigationLink(value: SettingsNavigationState.myNewFeature) {
    Label("My New Feature", systemImage: "star")
}
```

## 4. Den Deep Link behandeln (optional)
Wenn Sie eine tiefe Verbindung benötigen (`meshtastic:///settings/myNewFeature`), Handhabung hinzufügen in`Router.routeSettings()`:
```swift
func routeSettings(path: String) {
    if let state = SettingsNavigationState(rawValue: path) {
        navigationState.settingsNavigationState = state
        navigationState.selectedTab = .settings
    }
}
```

Die`rawValue`Init verarbeitet dies bereits automatisch für`String`-Gesicherte Enums - kein zusätzlicher Code erforderlich.
Dokumentieren Sie die neue URL in`docs/developer/deep-links.md`.
## 5. Protokollierung hinzufügen
Importieren Sie die entsprechende Loggerkategorie aus`Logger.swift`:
```swift
Logger.data.debug("MyNewFeatureView appeared")
```

Erstellen Sie nur dann eine neue Kategorie, wenn vorhandene Kategorien nicht passen. Verfügbare Kategorien finden Sie im [Codebase Guide](codebase.md).
## 6. Tests schreiben
Fügen Sie eine Testdatei in`MeshtasticTests/`Mit Swift Testing:
```swift
import Testing
@testable import Meshtastic

@Suite("MyNewFeatureTests")
struct MyNewFeatureTests {
    @Test func navigationCaseHasCorrectRawValue() {
        #expect(SettingsNavigationState.myNewFeature.rawValue == "myNewFeature")
    }
}
```

Führen Sie Tests in Xcode mit ⌘U durch, bevor Sie eine PR öffnen.
