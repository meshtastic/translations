
# Contribuer
Merci d'avoir contribué à Meshtastic Apple ! Veuillez lire ce guide avant d'ouvrir un PR.
## Prérequis
- Xcode (dernier stable)- Simulateurs iOS/macOS installés- SwiftLint (`brew install swiftlint`)
Exécutez `./scripts/setup-hooks.sh` une fois après le clonage pour installer le crochet SwiftLint pré-commit.
## Documentation
L'application est livrée avec un navigateur d'aide et de documentation intégré, un site Jekyll sur les pages GitHub, et la documentation est également publiée sur le site principal [meshtastic.org](https://meshtastic.org).
| Ressource | Emplacement |
|----------|----------|
| Meshtastic.org | [Meshtastic.org/docs/category/apple-apps](https://meshtastic.org/docs/category/apple-apps/) |
| Pages GitHub | [meshtastic.github.io/Meshtastic-Apple](https://meshtastic.github.io/Meshtastic-Apple/) |
| Dans l'application | Paramètres → Aide et documentation |
| Lien profond | [`meshtastic:///settings/helpDocs`](meshtastic:///settings/helpDocs) |

La démarque de la source vit sous `docs/user/` et `docs/developer/`. Pour reconstruire le HTML groupé après avoir modifié toute démarque :
```sh
bash scripts/build-docs.sh --output Meshtastic/Resources/docs --beta
```

Configez les fichiers régénérés sous `Meshtastic/Resources/docs/` avec votre PR.
## Nommage de la branche
Branche de `main` (développement basé sur le tronc). Utilisez des noms descriptifs :
```
feat/bluetooth-reconnect-improvements
fix/crash-on-ble-disconnect
docs/update-mqtt-guide
chore/update-protobufs
```

## Messages de validation
Utilisez les lignes d'objet de l'humeur impérative :
```
Fix crash when BLE device disconnects
Add TAK CoT position relay support
Update protobufs to v2.7
```

Expliquez *ce* qui a changé et *pourquoi* dans le corps. Gardez les lignes d'objet inférieures à 72 caractères.
## Liste de contrôle des relations publiques
- [ ] Tous les tests existants passent (`⌘U` dans Xcode)- [ ] Nouveaux tests écrits pour les nouvelles fonctionnalités et les corrections de bugs- [ ] SwiftLint ne signale aucune nouvelle erreur ou avertissement- [ ] Les changements d'interface utilisateur comprennent des captures d'écran ou un enregistrement d'écran dans la description des relations publiques- [ ] Les ajouts de liens profonds sont documentés dans `docs/developer/deep-links.md`- [ ] Les modifications du schéma SwiftData incluent un `VersionedSchema` et `MigrationStage`- [ ] Les changements de Protobuf sont régénérés avec `./scripts/gen_protos.sh` et construits
## Style de code
- **Swift uniquement. ** Aucun objectif-C.- **SwiftUI** pour toute l'interface utilisateur. UIKit seulement là où c'était inévitable.- **Symboles SF** pour toutes les icônes - pas d'actifs d'image intégrés pour les icônes.- **OSLog** pour tous les journaux - pas de `print()`. SwiftLint l'applique.- Indent avec **onglets**.- Ouverture des accolades sur la même ligne.- `// MARK : -` pour séparer les sections logiques.- `garde` pour une sortie anticipée ; évitez `if` profondément imbriqué.
## Limites de SwiftLint
| Vérifier | Avertissement | Erreur |
|-------|---------|-------|
| Longueur de la ligne | 400 | — |
| Longueur du fichier | 3500 | — |
| Type longueur du corps | 400 | — |
| Longueur du corps de fonction | 200 | — |
| Complexité cyclomatique | 60 | — |
| Type de nom longueur | 60 | 70 |

## Gardes de plate-forme
- Garder les API iOS uniquement : `#if ! Environnement cible (macCatalyst)`- Le gardien peut importer : `#if canImport(UIKit)`- Disponibilité de la version Guard : `if #available(iOS 26, *) { ... }`
## Mise à jour des Protobufs
1. `mise à jour du sous-module git --remote protobufs/`2. `./scripts/gen_protos.sh`3. Construire et vérifier la réussite des tests.4. Commit les modifications générées avec la mise à jour du pointeur du sous-module.
## Processus de libération
Voir `RELEASING.md` dans la racine du référentiel pour la liste de contrôle de publication complète et le processus de soumission de l'App Store.
