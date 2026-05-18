
# Tests

La cible du test est`MeshtasticTests/`. Tous les nouveaux tests doivent utiliser **Swift Testing** (`import Testing`).

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

- Utiliser`@Suite`Aux tests liés au groupe sous un nom descriptif.
- Utiliser`#expect`Pour les affirmations (ne lance pas d'échec - le test se poursuit).
- Utiliser`#require`Pour les conditions préalables (lance en cas d'échec - arrêts de test).
- N'utilisez pas`XCTAssert*`Dans de nouveaux fichiers de test.

## Tests d'exécution

Exécutez avec ⌘U dans Xcode. Il n'y a pas de test CLI - les tests nécessitent Xcode.

Assurez-vous que tous les tests existants réussissent avant d'ouvrir un PR. SwiftLint s'exécute à chaque validation ; les tests qui échouent en raison d'erreurs de lint bloqueront CI.

## Tests instantanés

Tests instantanés pour les vues SwiftUI en direct`MeshtasticTests/SwiftUIViewSnapshotTests.swift`.

### Comment fonctionnent les instantanés

1. A`renderImage`L'assistant rend une vue SwiftUI à un`UIImage`En utilisant`UIHostingController`+`drawHierarchy(in:afterScreenUpdates:true)`.
2. Lors de la première exécution, le PNG est enregistré comme référence. Instantanés avec`forDocs: true`Sont sauvegardés pour`docs/assets/screenshots/`(Partageé avec le site de documentation) ; les instantanés de test uniquement sont enregistrés dans`MeshtasticTests/__Snapshots__/`.
3. Lors d'exécutions suivantes, l'image rendue est comparée pixel par pixel à la référence en utilisant`CGImage`Dimensions.
4.`copy-snapshots.sh`Ne copie que les PNG référencés par des documents dans l'ensemble de l'application - les instantanés de test uniquement ne sont jamais groupés.

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

- Suites de noms`<ViewName>SnapshotTests`.
- Comparer en utilisant`cgImage.width`/`cgImage.height`(Dimensions de pixels à l'échelle de l'écran), pas`UIImage.size`(Qui dépend de l'échelle).
- Pour les vues avec`ScrollView`Ou pas de hauteur intrinsèque, passez un explicite`height:`Paramètre à`renderImage`.
- Commit les PNG de référence à côté du fichier de test.

### Intégration de paires d'instantanés sombres/clairs dans Docs

Lorsqu'une vue est snapshotée dans les deux schémas de couleurs (par ex.`foo_light.png`+`foo_dark.png`), Intégrant les deux`![]()`Les balises côte à côte font apparaître les deux images simultanément sur le site Jekyll et dans la visionneuse intégrée à l'application. Utiliser un HTML`<picture>`Élément à la place :

```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/foo_dark.png" />
  <img src="../assets/screenshots/foo_light.png" alt="Description" />
</picture>
```

Cela fonctionne dans les deux contextes parce que`build-docs.sh`Invoque`cmark-gfm --unsafe`(Le HTML brut est transmis) et`WKWebView`(Utilisé pour l'affichage dans l'application) est un WebKit complet et respecte`prefers-color-scheme`.

### Régénération des instantanés

Supprimez le PNG de référence et exécutez le test une fois - il enregistre une nouvelle référence. Engagez la nouvelle référence avec votre PR.

## Tests asynchrones

Pour les tests impliquant`async/await`:

```swift
@Test func asyncOperation() async throws {
    let result = await someAsyncFunction()
    #expect(result != nil)
}
```

Le routeur est`@MainActor`; Y accéder dans les tests avec`await MainActor.run { }`:

```swift
@Test func routerNavigates() async {
    let router = await MainActor.run { Router() }
    await MainActor.run { router.routeSettings(path: "helpDocs") }
    let state = await MainActor.run { router.navigationState.settingsNavigationState }
    #expect(state == .helpDocs)
}
```

