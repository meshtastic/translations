
# Contribuyendo

¡Gracias por contribuir a Meshtastic Apple! Por favor, lea esta guía antes de abrir un PR.

## Requisitos previos

- Xcode (estable más último)
- Simuladores iOS/macOS instalados
- SwiftLint (`brew install swiftlint`)

Correr`./scripts/setup-hooks.sh`Una vez después de la clonación para instalar el gancho SwiftLint previo a la conmición.

## Documentación

La aplicación incluye un navegador de Ayuda y Documentación incorporado, un sitio Jekyll en GitHub Pages, y la documentación también se publica en el principal[Meshtastic.org](https://meshtastic.org)Sitio.

| Fuente | Lugar |
|----------|----------|
| Meshtastic.org | [Meshtastic.org/docs/category/apple-apps](https://meshtastic.org/docs/category/apple-apps/) |
| Páginas de GitHub | [Meshtastic.github.io/Meshtastic-Apple](https://meshtastic.github.io/Meshtastic-Apple/) |
| En la aplicación | Configuración → Ayuda y documentación |
| Enlace profundo | [`meshtastic:///settings/helpDocs`](meshtastic:///settings/helpDocs) |

La rebaja de la fuente vive bajo`docs/user/`Y`docs/developer/`. Para reconstruir el HTML incluido después de editar cualquier rebaja:

```sh
bash scripts/build-docs.sh --output Meshtastic/Resources/docs
```

Confirmar los archivos regenerados bajo`Meshtastic/Resources/docs/`Con tu PR.

## Nombramiento de la sucursal

Rama de`main`(Desarrollo basado en el tronco). Utilice nombres descriptivos:

```
feat/bluetooth-reconnect-improvements
fix/crash-on-ble-disconnect
docs/update-mqtt-guide
chore/update-protobufs
```

## Confirmar mensajes

Use líneas de asunto de estado imperativo:

```
Fix crash when BLE device disconnects
Add TAK CoT position relay support
Update protobufs to v2.7
```

Explica *qué* ha cambiado y *por qué* en el cuerpo. Mantenga las líneas de asunto con menos de 72 caracteres.

## Lista de verificación de relaciones públicas

- [ ] Todas las pruebas existentes pasan (`⌘U`En Xcode)
- [ ] Nuevas pruebas escritas para nuevas características y correcciones de errores
- [ ] SwiftLint no informa de nuevos errores o advertencias
- [ ] Los cambios en la interfaz de usuario incluyen capturas de pantalla o una grabación de pantalla en la descripción de relaciones públicas
- [ ] Las adiciones de enlaces profundos están documentadas en`docs/developer/deep-links.md`
- [ ] Los cambios en el esquema de SwiftData incluyen un`VersionedSchema`Y`MigrationStage`
- [ ] Los cambios de Protobuf se regeneran con`./scripts/gen_protos.sh`Y construido

## Estilo de código

- **Solo Swift. ** Sin objetivo-C.
- **SwiftUI** para toda la interfaz de usuario. UIKit solo donde es inevitable.
- **Símbolos SF** para todos los iconos - no hay activos de imagen incrustados para los iconos.
- **OSLog** para todos los registros — no`print()`. SwiftLint hace cumplir esto.
- Indente con **pestañas**.
- Apertura de aparatos ortopédicos en la misma línea.
-`// MARK: -`Para separar secciones lógicas.
-`guard`Para una salida temprana; evite los profundamente anidados`if`.

## Límites de SwiftLint

| Cheque | Advertencia | Error informático |
|-------|---------|-------|
| Longitud de la línea | 400 | — |
| Longitud del archivo | 3500 | — |
| Tipo de longitud del cuerpo | 400 | — |
| Longitud del cuerpo de función | 200 | — |
| Complejidad ciclomática | 60 | — |
| Longitud del nombre del tipo | 60 | 70 |

## Guardiánes de plataforma

- Proteger las API solo para iOS:`#if !targetEnvironment(macCatalyst)`
- El guardia puede importar:`#if canImport(UIKit)`
- Disponibilidad de la versión Guard:`if #available(iOS 26, *) { ... }`

## Actualización de Protobufs

1.`git submodule update --remote protobufs/`
2.`./scripts/gen_protos.sh`
3. Construir y verificar que las pruebas pasen.
4. Confirmar los cambios generados junto con la actualización del puntero del submódulo.

## Proceso de liberación

Ver`RELEASING.md`En la raíz del repositorio para la lista de verificación de lanzamiento completa y el proceso de envío de la App Store.

