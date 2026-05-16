
# Aperçu de l'architecture

L'application Meshtastic Apple cible iOS, iPadOS, macOS (via Mac Catalyst), watchOS et visionOS. Il communique avec les radios Meshtastic sur BLE, TCP/IP et (sur macOS) en série.

## Point d'entrée de l'application

`Meshtastic/MeshtasticApp.swift`C'est le`@main` `App`Structure. Lors de son lancement :

1. Crée`PersistenceController.shared`(Données Swift`ModelContainer`)
2. Instancie`AppState`(Enveloppes`Router`)
3. Instancie`AccessoryManager`(Connectivité BLE/TCP/série)
4. Instancie`AccessoryManager.shared`En tant que`@EnvironmentObject`Pour la hiérarchie des vues

`MeshtasticAppDelegate.swift`Poignées`UIApplicationDelegate`Crochets pour les intentions de messagerie SiriKit CarPlay.

## Routeur et navigation

`Router`(`Meshtastic/Router/Router.swift`) Est un`@MainActor` `ObservableObject`Qui possède un`NavigationState`Structure. Il pilote la sélection des onglets et le routage des liens profonds.

```
Router
└── NavigationState
    ├── MessagesNavigationState   (tab 0)
    ├── MapNavigationState        (tab 1)
    ├── NodesNavigationState      (tab 2)
    └── SettingsNavigationState   (tab 3)
```

Les liens profonds utilisent le`meshtastic:///`Schéma d'URL.`Router.route(url:)`Analyse le chemin et définit l'état de navigation approprié. Voir [Deep Links](deep-links) pour la référence complète de l'URL.

## État de l'application

`AppState`Enveloppements`Router`Et est injecté comme un`@EnvironmentObject`À la racine de la hiérarchie des vues SwiftUI. Vues qui doivent naviguer de manière programmatique`@EnvironmentObject var router: Router`Directement - ou plus communément`@EnvironmentObject var appState: AppState`Et accès`appState.router`.

## Gestionnaire d'accessoires

`AccessoryManager`Est le gestionnaire de connectivité central divisé entre les fichiers d'extension :

| Fichier | Responsabilité |
|------|---------------|
| `AccessoryManager+Discovery.swift` | Balayage BLE, découverte d'appareils |
| `AccessoryManager+Connect.swift` | Cycle de vie de la connexion, logique de reconnexion |
| `AccessoryManager+ToRadio.swift` | Paquets envoyés à la radio |
| `AccessoryManager+FromRadio.swift` | Paquets reçus de la radio |
| `AccessoryManager+Position.swift` | Partage de position GPS |
| `Gestionnaire d'accessoires+MQTT.swift` | Proxy MQTT |
| `AccessoryManager+TAK.swift` | Intégration TAK/CoT |

Les protocoles de transport sont en`Meshtastic/Accessory/Transports/`.

## Persistance

SwiftData est la seule couche de persistance.`PersistenceController.shared`Possède le`ModelContainer`. Les vues utilisent`@Environment(\.modelContext)`Ou`@Query`. Les écritures d'arrière-plan utilisent le`MeshPackets` `@ModelActor`.

Les types de modèles sont définis avec`@Model`Dans`Meshtastic/Model/`. Utilisations de l'évolution du schéma`VersionedSchema`Et`SchemaMigrationPlan`Dans`MeshtasticSchema.swift`.

## Services

Les services d'application qui ne sont pas liés à la connectivité radio en direct`Meshtastic/Services/`.

| Fichier | Responsabilité |
|------|---------------|
| `DocTranslationService.swift` | Traduction de la documentation sur l'appareil à l'aide du cadre de traduction Apple (principal) avec le repli FoundationModels. Traduit les fichiers sources de markdown anglais groupés, met en cache la traduction `.md`, convertit en HTML via `MarkdownConverter` et déclenche le téléchargement automatique après la prélecture. iOS 26+. |
| `TraductionCache.swift` | Cache basé sur des fichiers pour le contenu `.md` traduit stocké dans le support de l'application. Suit les hachages de contenu pour la détection de la sténure et applique une politique d'expulsion LRU de 50 Mo par langue. |
| `MarkdownConverter.swift` | Markdown→Convertisseur HTML compatible GFM. Prend en charge les titres, les paragraphes, les listes, les clôtures de code, le code en ligne, les tableaux, les liens, les images, le passage HTML (`<picture>`, `<img>`), les callouts blockquote (astuce/avertissement), gras, italique, barré, règles horizontales et `.md` → `.html` la réécriture de lien. Dénude la matière avant YAML et les attributs en ligne Jekyll. |
| `DocsTranslationUploader.swift` | Commit automatiquement les fichiers `.md` traduits au dépôt `meshtastic/translations` après la fin du préretch d'arrière-plan. Effectue des vérifications en lecture seule par rapport à `meshtastic/meshtastic` et `meshtastic/translations` (pas d'authentification), puis s'engage via l'API GitHub Contents à l'aide d'un PAT à grain fin de `Secrets.json`. Le suivi par fichier permet de réessayer les téléchargements ayant échoué. |

## Protobufs

Le`MeshtasticProtobufs`Paquet Swift (`MeshtasticProtobufs/Package.swift`) Enveloppe les sources Swift générées par protobuf. Régénérer avec`./scripts/gen_protos.sh`Après avoir mis à jour le`protobufs/`Sous-module.

