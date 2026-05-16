
# Testlauf

Das Testziel ist`MeshtasticTests/`. Alle neuen Tests müssen **Swift Testing** verwenden (`import Testing`).

## Schreibtests

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

- Verwenden`@Suite`Um verwandte Tests unter einem beschreibenden Namen zu gruppieren.
- Verwenden`#expect`Für Behauptungen (wirft keinen Fehler - Test wird fortgesetzt).
- Verwenden`#require`Für Voraussetzungen (wirft Fehler - Teststopps).
- Verwenden du nicht`XCTAssert*`In neuen Testdateien.

## Laufende Tests

Mit ⌘U in Xcode ausführen. Es gibt keinen CLI-Testläufer - Tests erfordern Xcode.

Stellen du sicher, dass alle vorhandenen Tests bestehen, bevor du eine PR öffnen. SwiftLint wird bei jedem Commit ausgeführt; Tests, die aufgrund von Lint-Fehlern fehllaufen, blockieren CI.

## Snapshot-Tests

Snapshot-Tests für SwiftUI-Ansichten live in`MeshtasticTests/SwiftUIViewSnapshotTests.swift`.

### Wie Schnappschüsse funktionieren

1. A`renderImage`Helfer rendert eine SwiftUI-Ansicht zu einem`UIImage`Mit`UIHostingController`+`drawHierarchy(in:afterScreenUpdates:true)`.
2. Beim ersten Lauf wird der PNG als Referenz gespeichert. Schnappschüsse mit`forDocs: true`Werden gespeichert zu`docs/assets/screenshots/`(Mit der Dokumentationsseite geteilt); Nur-Test-Snapshots werden gespeichert in`MeshtasticTests/__Snapshots__/`.
3. Bei nachfolgenden Laufläufen wird das gerenderte Bild Pixel für Pixel mit der Referenz unter Verwendung von`CGImage`Abmessungen.
4.`copy-snapshots.sh`Kopiert nur dokumentbezogene PNGs in das App-Bundle - nur testbezogene Schnappschüsse werden niemals gebündelt.

### Einen Schnappschusstest schreiben

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

- Namenssuiten`<ViewName>SnapshotTests`.
- Vergleichen du mit`cgImage.width`/`cgImage.height`(Pixelabmessungen im Bildschirmmaßstab), nicht`UIImage.size`(Was maßstabsabsatsabhängig ist).
- Für Ansichten mit`ScrollView`Oder keine intrinsische Höhe, bestehen du eine explizite`height:`Parameter zu`renderImage`.
- Verweisen du PNGs neben der Testdatei.

### Einbetten von dunklen/hellen Snapshot-Paaren in Dokumente

Wenn eine Ansicht in beiden Farbschemata (z.B.`foo_light.png`+`foo_dark.png`), Einbettet beide`![]()`Tags nebeneinander führen dazu, dass beide Bilder gleichzeitig auf der Jekyll-Site und im In-App-Viewer angezeigt werden. Verwenden du ein HTML`<picture>`Element stattdessen:

```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/foo_dark.png" />
  <img src="../assets/screenshots/foo_light.png" alt="Description" />
</picture>
```

Dies funktioniert in beiden Zusammenhängen, weil`build-docs.sh`Beschwört`cmark-gfm --unsafe`(Rohes HTML wird weitergegeben) und`WKWebView`(Wird für die In-App-Anzeige verwendet) ist voll WebKit und respektiert`prefers-color-scheme`.

### Snapshots regenerieren

Löschen du den Referenz-PNG und führen du den Test einmal durch – er zeichnet eine neue Referenz auf. Verpflichten du die neue Referenz mit deiner PR.

## Asynchrone Tests

Für Tests mit`async/await`:

```swift
@Test func asyncOperation() async throws {
    let result = await someAsyncFunction()
    #expect(result != nil)
}
```

Router ist`@MainActor`; Zugriff darauf in Tests mit`await MainActor.run { }`:

```swift
@Test func routerNavigates() async {
    let router = await MainActor.run { Router() }
    await MainActor.run { router.routeSettings(path: "helpDocs") }
    let state = await MainActor.run { router.navigationState.settingsNavigationState }
    #expect(state == .helpDocs)
}
```

