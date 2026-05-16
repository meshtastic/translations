
# Pruebas

El objetivo de prueba es`MeshtasticTests/`. Todas las pruebas nuevas deben usar **Swift Testing** (`import Testing`).

## Pruebas de escritura

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

- Uso`@Suite`Para grupos de pruebas relacionadas con un nombre descriptivo.
- Uso`#expect`Para afirmaciones (no lanza el fracaso - la prueba continúa).
- Uso`#require`Para condiciones previas (lanza en fallo - paradas de prueba).
- No usar`XCTAssert*`En nuevos archivos de prueba.

## Pruebas de ejecución

Ejecuta con ⌘U en Xcode. No hay un ejecutor de pruebas CLI - las pruebas requieren Xcode.

Asegúrese de que todas las pruebas existentes pasen antes de abrir un PR. SwiftLint se ejecuta en cada confirmación; las pruebas que fallan debido a errores de pelusa bloquearán CI.

## Pruebas instantáneas

Pruebas instantáneas para las vistas de SwiftUI en vivo`MeshtasticTests/SwiftUIViewSnapshotTests.swift`.

### Cómo funcionan las instantáneas

1. A`renderImage`El ayudante representa una vista de SwiftUI a un`UIImage`Usando`UIHostingController`+`drawHierarchy(in:afterScreenUpdates:true)`.
2. En la primera ejecución, el PNG se guarda como referencia. Instantáneas con`forDocs: true`Se guardan para`docs/assets/screenshots/`(Compartido con el sitio de documentación); las instantáneas de solo prueba se guardan en`MeshtasticTests/__Snapshots__/`.
3. En ejecuciones posteriores, la imagen renderizada se compara píxel por píxel con la referencia usando`CGImage`Dimensiones.
4.`copy-snapshots.sh`Copia solo los PNG con referencia a documentos en el paquete de aplicaciones: las instantáneas de solo pruebas nunca se agrupan.

### Escribir una prueba de instantánea

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

- Nombre de las suites`<ViewName>SnapshotTests`.
- Comparar usando`cgImage.width`/`cgImage.height`(Dimensiones de píxeles a escala de pantalla), no`UIImage.size`(Que depende de la escala).
- Para vistas con`ScrollView`O sin altura intrínseca, pasa un explícito`height:`Parámetro a`renderImage`.
- Confirmar PNG de referencia junto al archivo de prueba.

### Incrustación de pares de instantáneas oscuras/ligeras en Docs

Cuando una vista se instantánea en ambos esquemas de color (p. ej.`foo_light.png`+`foo_dark.png`), Incrustando ambos`![]()`Las etiquetas una al lado de la otra hacen que ambas imágenes aparezcan simultáneamente en el sitio de Jekyll y en el visor de la aplicación. Usa un HTML`<picture>`Elemento en su lugar:

```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/foo_dark.png" />
  <img src="../assets/screenshots/foo_light.png" alt="Description" />
</picture>
```

Esto funciona en ambos contextos porque`build-docs.sh`Invoca`cmark-gfm --unsafe`(Se pasa HTML en bruto) y`WKWebView`(Utilizado para la visualización en la aplicación) es WebKit completo y respeta`prefers-color-scheme`.

### Regeneración de instantáneas

Elimine el PNG de referencia y ejecute la prueba una vez; registra una nueva referencia. Compromete la nueva referencia con su PR.

## Pruebas asíncronas

Para pruebas que involucran`async/await`:

```swift
@Test func asyncOperation() async throws {
    let result = await someAsyncFunction()
    #expect(result != nil)
}
```

Enrutador es`@MainActor`; Accede a él en pruebas con`await MainActor.run { }`:

```swift
@Test func routerNavigates() async {
    let router = await MainActor.run { Router() }
    await MainActor.run { router.routeSettings(path: "helpDocs") }
    let state = await MainActor.run { router.navigationState.settingsNavigationState }
    #expect(state == .helpDocs)
}
```

