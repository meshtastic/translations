
# Ajout de fonctionnalités

Ce guide suit l'ajout d'une nouvelle vue de paramètres de bout en bout : état de navigation, lien profond, fichier d'affichage et intégration des paramètres.

## 1. Ajouter un étui de navigation

Ouvert`Meshtastic/Router/NavigationState.swift`Et ajoutez un nouveau dossier à`SettingsNavigationState`:

```swift
enum SettingsNavigationState: String {
    // ... existing cases ...
    case myNewFeature   // raw value is "myNewFeature" — matches the deep link path segment
}
```

Utiliser`lowerCamelCase`Valeurs brutes. La valeur brute devient le segment de chemin d'URL pour la liaison profonde.

## 2. Créer la vue

Créez un nouveau fichier SwiftUI sous`Meshtastic/Views/Settings/`:

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

Gardez les fichiers de visualisation concentrés. Si la vue dépasse ~400 lignes (avertissement SwiftLint), divisez-la en sous-vues ou en fichiers d'extension.

## 3. Navigation des paramètres du câblage

Ouvert`Meshtastic/Views/Settings/Settings.swift`Et trouve le`navigationDestination(for:)`Commutateur. Ajoutez votre dossier :

```swift
.navigationDestination(for: SettingsNavigationState.self) { state in
    switch state {
    // ... existing cases ...
    case .myNewFeature:
        MyNewFeatureView()
    }
}
```

Ensuite, ajoutez un`NavigationLink`Dans la section appropriée de la liste Paramètres :

```swift
NavigationLink(value: SettingsNavigationState.myNewFeature) {
    Label("My New Feature", systemImage: "star")
}
```

## 4. Gérer le lien profond (facultatif)

Si vous avez besoin d'un lien profond (`meshtastic:///settings/myNewFeature`), Ajouter la manipulation dans`Router.routeSettings()`:

```swift
func routeSettings(path: String) {
    if let state = SettingsNavigationState(rawValue: path) {
        navigationState.settingsNavigationState = state
        navigationState.selectedTab = .settings
    }
}
```

Le`rawValue`Init gère déjà cela automatiquement pour`String`-Enums soutenus - aucun code supplémentaire nécessaire.

Documentez la nouvelle URL dans`docs/developer/deep-links.md`.

## 5. Ajouter la journalisation

Importez la catégorie d'enregistreur appropriée à partir de`Logger.swift`:

```swift
Logger.data.debug("MyNewFeatureView appeared")
```

Créez une nouvelle catégorie uniquement si les catégories existantes ne correspondent pas. Consultez le [Guide Codebase](codebase.md) pour les catégories disponibles.

## 6. Écrire des tests

Ajouter un fichier de test dans`MeshtasticTests/`En utilisant Swift Testing :

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

Exécutez des tests dans Xcode avec ⌘U avant d'ouvrir un PR.

