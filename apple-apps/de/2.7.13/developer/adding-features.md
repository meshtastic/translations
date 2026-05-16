
# Hinzufügen von Funktionen

Dieser Leitfaden führt du durch das Hinzufügen einer neuen Einstellungsansicht von Ende zu Ende: Navigationsstatus, Deep Link, Ansichtsdatei und Integration von Einstellungen.

## 1. Navigationsgehäuse hinzufügen

Offen`Meshtastic/Router/NavigationState.swift`Und fügen du einen neuen Fall zu`SettingsNavigationState`:

```swift
enum SettingsNavigationState: String {
    // ... existing cases ...
    case myNewFeature   // raw value is "myNewFeature" — matches the deep link path segment
}
```

Verwenden`lowerCamelCase`Rohwerte. Der Rohwert wird zum URL-Pfadsegment für Deep Linking.

## 2. Erstellen du die Ansicht

Erstellen du eine neue SwiftUI-Datei unter`Meshtastic/Views/Settings/`:

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

Halten du die Ansichtsdateien fokussiert. Wenn die Ansicht über ~400 Zeilen hinausgeht (SwiftLint-Warnung), teilen du sie in Unteransichten oder Erweiterungsdateien auf.

## 3. Navigation "Einstellungen" verdrahten

Offen`Meshtastic/Views/Settings/Settings.swift`Und finde die`navigationDestination(for:)`Schalter. Fügen du deinen Fall hinzu:

```swift
.navigationDestination(for: SettingsNavigationState.self) { state in
    switch state {
    // ... existing cases ...
    case .myNewFeature:
        MyNewFeatureView()
    }
}
```

Fügen du dann eine`NavigationLink`Im entsprechenden Abschnitt der Einstellungsliste:

```swift
NavigationLink(value: SettingsNavigationState.myNewFeature) {
    Label("My New Feature", systemImage: "star")
}
```

## 4. Den Deep Link behandeln (optional)

Wenn du eine tiefe Verbindung benötigen (`meshtastic:///settings/myNewFeature`), Handhabung hinzufügen in`Router.routeSettings()`:

```swift
func routeSettings(path: String) {
    if let state = SettingsNavigationState(rawValue: path) {
        navigationState.settingsNavigationState = state
        navigationState.selectedTab = .settings
    }
}
```

Die`rawValue`Init verarbeitet dies bereits automatisch für`String`-Gesicherte Enums - kein zusätzlicher Code erforderlich.

Dokumentieren du die neue URL in`docs/developer/deep-links.md`.

## 5. Protokollierung hinzufügen

Importieren du die entsprechende Loggerkategorie aus`Logger.swift`:

```swift
Logger.data.debug("MyNewFeatureView appeared")
```

Erstellen du nur dann eine neue Kategorie, wenn vorhandene Kategorien nicht passen. Verfügbare Kategorien finden du im [Codebase Guide](codebase.md).

## 6. Tests schreiben

Fügen du eine Testdatei in`MeshtasticTests/`Mit Swift Testing:

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

Führen du Tests in Xcode mit ⌘U durch, bevor du eine PR öffnen.

