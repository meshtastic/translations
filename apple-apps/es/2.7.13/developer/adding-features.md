
# Adición de características

Esta guía guía a través de la adición de una nueva vista de configuración de extremo a extremo: estado de navegación, enlace profundo, archivo de vista e integración de Configuración.

## 1. Añadir un caso de navegación

Abierto`Meshtastic/Router/NavigationState.swift`Y añade un nuevo caso a`SettingsNavigationState`:

```swift
enum SettingsNavigationState: String {
    // ... existing cases ...
    case myNewFeature   // raw value is "myNewFeature" — matches the deep link path segment
}
```

Uso`lowerCamelCase`Valores brutos. El valor bruto se convierte en el segmento de la ruta de la URL para el enlace profundo.

## 2. Crear la vista

Crea un nuevo archivo SwiftUI en`Meshtastic/Views/Settings/`:

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

Mantenga los archivos de visualización enfocados. Si la vista crece más allá de ~400 líneas (advertencia de SwiftLint), divídala en subvistas o archivos de extensión.

## 3. Navegación de configuración de cableado

Abierto`Meshtastic/Views/Settings/Settings.swift`Y encuentra el`navigationDestination(for:)`Cambiar. Añade tu caso:

```swift
.navigationDestination(for: SettingsNavigationState.self) { state in
    switch state {
    // ... existing cases ...
    case .myNewFeature:
        MyNewFeatureView()
    }
}
```

Luego añade un`NavigationLink`En la sección correspondiente de la lista de Ajustes:

```swift
NavigationLink(value: SettingsNavigationState.myNewFeature) {
    Label("My New Feature", systemImage: "star")
}
```

## 4. Manejar el enlace profundo (opcional)

Si necesitas un enlace profundo (`meshtastic:///settings/myNewFeature`), Añadir manejo en`Router.routeSettings()`:

```swift
func routeSettings(path: String) {
    if let state = SettingsNavigationState(rawValue: path) {
        navigationState.settingsNavigationState = state
        navigationState.selectedTab = .settings
    }
}
```

El`rawValue`Init ya maneja esto automáticamente para`String`-Enums respaldados - no se necesita código adicional.

Documenta la nueva URL en`docs/developer/deep-links.md`.

## 5. Añadir registro

Importe la categoría de registrador apropiada desde`Logger.swift`:

```swift
Logger.data.debug("MyNewFeatureView appeared")
```

Cree una nueva categoría solo si las categorías existentes no encajan. Ver el[Guía de base de código](codebase.md)Para las categorías disponibles.

## 6. Pruebas de escritura

Añade un archivo de prueba en`MeshtasticTests/`Usando Swift Testing:

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

Ejecute pruebas en Xcode con ⌘U antes de abrir un PR.

