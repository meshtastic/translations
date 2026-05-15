
# Beitragen
Vielen Dank für Ihren Beitrag zu Meshtastic Apple! Bitte lesen Sie diesen Leitfaden, bevor Sie eine PR eröffnen.
## Voraussetzungen
- Xcode (neueste stabile)- IOS/macOS-Simulatoren installiert- SwiftLint (`brew install swiftlint`)
Laufen`./scripts/setup-hooks.sh`Einmal nach dem Klonen, um den SwiftLint-Haken vor dem Commit zu installieren.
## Dokumentation
Die App bietet einen integrierten Hilfe- und Dokumentationsbrowser, eine Jekyll-Site auf GitHub Pages und die Dokumentation wird auch auf der Hauptseite [meshtastic.org](https://meshtastic.org) veröffentlicht.
| Hilfsmittel | Ort |
|----------|----------|
| Meshtastic.org | [Meshtastic.org/docs/category/apple-apps](https://meshtastic.orgorg/docs/category/apple-apps/) |
| GitHub-Seiten | [meshtastic.github.io/Meshtastic-Apple](https://meshtastic.github.io/Meshtastic-Apple/) |
| In-App | Einstellungen → Hilfe & Dokumentation |
| Tiefe Verbindung | [`meshtastic:///settings/helpDocs`](meshtastic:///settings/helpDocs) |

Quellenabschlag lebt unter`docs/user/`Und`docs/developer/`. Um den gebündelten HTML nach dem Bearbeiten eines Markdowns neu zu erstellen:
```sh
bash scripts/build-docs.sh --output Meshtastic/Resources/docs --beta
```

Commit die regenerierten Dateien unter`Meshtastic/Resources/docs/`Mit Ihrer PR.
## Zweignennung
Zweig von`main`(Trunk-basierte Entwicklung). Beschreibende Namen verwenden:
```
feat/bluetooth-reconnect-improvements
fix/crash-on-ble-disconnect
docs/update-mqtt-guide
chore/update-protobufs
```

## Commit-Nachrichten
Verwenden Sie Imperativ-Stimmungs-Subjektzeilen:
```
Fix crash when BLE device disconnects
Add TAK CoT position relay support
Update protobufs to v2.7
```

Erklären Sie *was* sich im Körper geändert hat und *warum*. Halten Sie die Betreffzeilen unter 72 Zeichen.
## PR-Checkliste
- [ ] Alle bestehenden Tests bestehen (`⌘U`In Xcode)- [ ] Neue Tests für neue Funktionen und Fehlerbehebungen- [ ] SwiftLint meldet keine neuen Fehler oder Warnungen- [ ] UI-Änderungen beinhalten Screenshots oder eine Bildschirmaufnahme in der PR-Beschreibung- [ ] Deep-Link-Ergänzungen sind dokumentiert in`docs/developer/deep-links.md`
- [ ] SwiftData-Schema-Änderungen beinhalten eine`VersionedSchema`Und`MigrationStage`
- [ ] Protobuf-Änderungen werden regeneriert mit`./scripts/gen_protos.sh`Und gebaut
## Code-Stil
- **Nur schnell. ** Kein Ziel-C.- **SwiftUI** für alle UI. UIKit nur dort, wo es unvermeidlich ist.- **SF-Symbole** für alle Symbole - keine eingebetteten Bildressourcen für Symbole.- **OSLog** für alle Protokollierungen - nein`print()`. SwiftLint setzt dies durch.- Einzug mit **Registerkarten**.- Öffnungsklammern an der gleichen Linie.-`// MARK: -`Um logische Abschnitte zu trennen.-`guard`Für einen frühen Ausgang; vermeiden Sie tief verschachtelte`if`.
## SwiftLint-Grenzen
| Scheck | Warnung | Fehler |
|-------|---------|-------|
| Linienlänge | 400 | — |
| Dateilänge | 3500 | — |
| Körperlänge vom Typ | 400 | — |
| Funktionskörperlänge | 200 | — |
| Zyklomatische Komplexität | 60 | — |
| Typ Name Länge | 60 | 70 |

## Plattformwächter
- Bewachen Sie nur iOS-APIs:`#if !targetEnvironment(macCatalyst)`
- Wachmann kann importieren:`#if canImport(UIKit)`
- Verfügbarkeit der Guard-Version:`if #available(iOS 26, *) { ... }`

## Protobufs aktualisieren
1.`git submodule update --remote protobufs/`
2.`./scripts/gen_protos.sh`
3. Erstellen und verifizieren Sie, ob Tests bestanden werden.4. Commit generierte Änderungen zusammen mit dem Submodul-Zeiger-Update.
## Freigabeprozess
Sehen`RELEASING.md`Im Repository-Root für die vollständige Veröffentlichungs-Checkliste und den App Store-Einreichungsprozess.
