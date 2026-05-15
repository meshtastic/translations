
# Test
La cible de test est `MeshtasticTests/`. Tous les nouveaux tests doivent utiliser **Swift Testing** (`import Testing`).
## Tests d'écriture
```swift
import Testing
@testable import Meshtastic

@Suite("MyFeatureTests")
struct MyFeatureTests {

    @Test func someExpectation() {
        let value = computeSomething()
        #expect(value == 42)
    }

    @Test func requiredValue() throws {
        let result = try #require(optionalValue())
        #expect(result.count > 0)
    }
}
```

- Utilisez `@Suite` pour regrouper les tests connexes sous un nom descriptif.- Utilisez `#expect` pour les affirmations (ne lance pas d'échec - le test se poursuit).- Utilisez `#require` pour les conditions préalables (lance sur échec - arrêts de test).- N'utilisez pas `XCTAssert*` dans les nouveaux fichiers de test.
## Tests en cours
Exécutez avec ⌘U dans Xcode. Il n'y a pas de test CLI - les tests nécessitent Xcode.
Assurez-vous que tous les tests existants réussissent avant d'ouvrir un PR. SwiftLint s'exécute à chaque validation ; les tests qui échouent en raison d'erreurs de lint bloqueront CI.
## Tests instantanés
Tests instantanés pour les vues SwiftUI en direct dans `MeshtasticTests/SwiftUIViewSnapshotTests.swift`.
### Comment fonctionnent les instantanés
1. Une aide `renderImage` rend une vue SwiftUI à une `UIImage` à l'aide de `UIHostingController` + `drawHierarchy(in:afterScreenUpdates:true)`.2. Lors de la première exécution, le PNG est enregistré comme référence. Les instantanés avec `forDocs : true` sont enregistrés dans `docs/assets/screenshots/` (partagé avec le site de documentation) ; les instantanés de test uniquement sont enregistrés dans `MeshtasticTests/__Snapshots__/`.3. Lors d'exécutions ultérieures, l'image rendue est comparée pixel par pixel à la référence en utilisant les dimensions `CGImage`.4. `copy-snapshots.sh` copie uniquement les PNG référencés par document dans l'ensemble de l'application - les instantanés de test uniquement ne sont jamais regroupés.
### Rédaction d'un test instantané
```swift
@Suite("MyViewSnapshotTests")
struct MyViewSnapshotTests {

    @Test func rendersCorrectly() throws {
        let image = try renderImage(MyView(), width: 390)
        let cgImage = try #require(image.cgImage)
        #expect(cgImage.width == 390 * Int(UIScreen.main.scale))
    }
}
```

- Nommez les suites `<ViewName>SnapshotTests`.- Comparez en utilisant `cgImage.width` / `cgImage.height` (dimensions des pixels à l'échelle de l'écran), et non `UIImage.size` (qui dépend de l'échelle).- Pour les vues avec `ScrollView` ou sans hauteur intrinsèque, passez un paramètre explicite `height:` à `renderImage`.- Commit les PNG de référence à côté du fichier de test.
### Intégration de paires d'instantanés sombres/clairs dans Docs
Lorsqu'une vue est snapshotée dans les deux schémas de couleurs (par exemple `foo_light.png` + `foo_dark.png`), l'intégration des deux `![]() ` les balises côte à côte font apparaître simultanément les deux images sur le site Jekyll et dans la visionneuse intégrée à l'application. Utilisez un élément HTML `<picture>` à la place :
```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/foo_dark.png" />
  <img src="../assets/screenshots/foo_light.png" alt="Description" />
</picture>
```

Cela fonctionne dans les deux contextes parce que `build-docs.sh` invoque `cmark-gfm --unsafe` (le HTML brut est transmis) et `WKWebView` (utilisé pour l'affichage dans l'application) est un WebKit complet et respecte `prefers-color-scheme`.
### Régénération des instantanés
Supprimez le PNG de référence et exécutez le test une fois - il enregistre une nouvelle référence. Engagez la nouvelle référence avec votre PR.
## Tests asynchrones
Pour les tests impliquant `async/await` :
```swift
@Test func asyncOperation() async throws {
    let result = await someAsyncFunction()
    #expect(result != nil)
}
```

Le routeur est `@MainActor` ; accédez-y dans les tests avec `await MainActor.run { }` :
```swift
@Test func routerNavigates() async {
    let router = await MainActor.run { Router() }
    await MainActor.run { router.routeSettings(path: "helpDocs") }
    let state = await MainActor.run { router.navigationState.settingsNavigationState }
    #expect(state == .helpDocs)
}
```

