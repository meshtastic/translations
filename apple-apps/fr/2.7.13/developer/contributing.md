
# Contribuer

Merci d'avoir contribué à Meshtastic Apple ! Veuillez lire ce guide avant d'ouvrir un PR.

## Prérequis

- Xcode (dernier stable)
- Simulateurs iOS/macOS installés
- SwiftLint (`brew install swiftlint`)

Courir`./scripts/setup-hooks.sh`Une fois après le clonage pour installer le crochet SwiftLint pré-commit.

## Documentation

L'application est livrée avec un navigateur d'aide et de documentation intégré, un site Jekyll sur les pages GitHub, et la documentation est également publiée sur le site principal [meshtastic.org](https://meshtastic.org).

| Ressource | Emplacement |
|----------|----------|
| Meshtastic.org | [Meshtastic.org/docs/category/apple-apps](https://meshtastic.org/docs/category/apple-apps/) |
| Pages GitHub | [meshtastic.github.io/Meshtastic-Apple](https://meshtastic.github.io/Meshtastic-Apple/) |
| Dans l'application | Paramètres → Aide et documentation |
| Lien profond | [`meshtastic:///settings/helpDocs`](meshtastic:///paramètres/helpDocs) |

La réduction de la source vit sous`docs/user/`Et`docs/developer/`. Pour reconstruire le HTML groupé après avoir modifié toute démarque :

```sh
bash scripts/build-docs.sh --output Meshtastic/Resources/docs --beta
```

Commit les fichiers régénérés sous`Meshtastic/Resources/docs/`Avec votre PR.

## Nommage des succursales

Branche de`main`(Développement basé sur le tronc). Utilisez des noms descriptifs :

```
feat/bluetooth-reconnect-improvements
fix/crash-on-ble-disconnect
docs/update-mqtt-guide
chore/update-protobufs
```

## Commit Messages

Utilisez les lignes d'objet de l'humeur impérative :

```
Fix crash when BLE device disconnects
Add TAK CoT position relay support
Update protobufs to v2.7
```

Expliquez *ce* qui a changé et *pourquoi* dans le corps. Gardez les lignes d'objet inférieures à 72 caractères.

## Liste de contrôle des relations publiques

- [ ] Tous les tests existants réussis (`⌘U`Dans Xcode)
- [ ] Nouveaux tests écrits pour les nouvelles fonctionnalités et les corrections de bugs
- [ ] SwiftLint ne signale aucune nouvelle erreur ou avertissement
- [ ] Les changements d'interface utilisateur comprennent des captures d'écran ou un enregistrement d'écran dans la description des relations publiques
- [ ] Les ajouts de liens profonds sont documentés dans`docs/developer/deep-links.md`
- [ ] Les changements de schéma SwiftData incluent un`VersionedSchema`Et`MigrationStage`
- [ ] Les changements de protobuf sont régénérés avec`./scripts/gen_protos.sh`Et construit

## Style de code

- **Swift uniquement. ** Aucun objectif-C.
- **SwiftUI** pour toute l'interface utilisateur. UIKit seulement là où c'était inévitable.
- **Symboles SF** pour toutes les icônes - pas d'actifs d'image intégrés pour les icônes.
- **OSLog** pour toute la journalisation - non`print()`. SwiftLint l'applique.
- Indent avec **onglets**.
- Ouverture des accolades sur la même ligne.
-`// MARK: -`Pour séparer les sections logiques.
-`guard`Pour une sortie anticipée ; évitez d'être profondément imbriqué`if`.

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

- Garder les API iOS uniquement :`#if !targetEnvironment(macCatalyst)`
- La garde peut importer :`#if canImport(UIKit)`
- Disponibilité de la version Guard :`if #available(iOS 26, *) { ... }`

## Mise à jour des protobufs

1.`git submodule update --remote protobufs/`
2.`./scripts/gen_protos.sh`
3. Construire et vérifier la réussite des tests.
4. Commit les modifications générées avec la mise à jour du pointeur du sous-module.

## Processus de libération

Voir`RELEASING.md`Dans la racine du référentiel pour la liste de contrôle complète de la version et le processus de soumission de l'App Store.

