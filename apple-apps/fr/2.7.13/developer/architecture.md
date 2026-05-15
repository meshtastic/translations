
# Aperçu de l'architecture
L'application Meshtastic Apple cible iOS, iPadOS et macOS (via Mac Catalyst). Il communique avec les radios Meshtastic sur BLE, TCP/IP et (sur macOS) en série.
## Point d'entrée de l'application
`Meshtastic/MeshtasticApp.swift` est la structure `@main` `App`. Lors de son lancement :
1. Crée `PersistenceController.shared` (SwiftData `ModelContainer`)2. Instancie `AppState` (enveloppe `Router`)3. Instancie `AccessoryManager` (Connectivité BLE/TCP/série)4. Instancie `AccessoryManager.shared` en tant que `@EnvironmentObject` pour la hiérarchie des vues
`MeshtasticAppDelegate.swift` gère les crochets `UIApplicationDelegate` pour les intentions de messagerie SiriKit CarPlay.
## Routeur et navigation
`Router` (`Meshtastic/Router/Router.swift`) est un `@MainActor` `ObservableObject` qui possède une structure `NavigationState`. Il pilote la sélection des onglets et le routage des liens profonds.
```
Router
└── NavigationState
    ├── MessagesNavigationState   (tab 0)
    ├── MapNavigationState        (tab 1)
    ├── NodesNavigationState      (tab 2)
    └── SettingsNavigationState   (tab 3)
```

Les liens profonds utilisent le schéma d'URL `meshtastic:///`. `Router.route(url:)` analyse le chemin et définit l'état de navigation approprié. Voir [Deep Links](deep-links) pour la référence complète de l'URL.
## État de l'application
`AppState` enveloppe `Router` et est injecté en tant que `@EnvironmentObject` à la racine de la hiérarchie des vues SwiftUI. Les vues qui doivent naviguer par programmation, lisez directement `@EnvironmentObject var router : Router` - ou plus communément `@EnvironmentObject var appState : AppState` et accédez à `appState.router`.
## Gestionnaire d'accessoires
`AccessoryManager` est le gestionnaire de connectivité central divisé entre les fichiers d'extension :
| Fichier | Responsabilité |
|------|---------------|
| `AccessoryManager+Discovery.swift` | Balayage BLE, découverte d'appareils |
| `AccessoryManager+Connect.swift` | Cycle de vie de la connexion, logique de reconnexion |
| `AccessoryManager+ToRadio.swift` | Paquets envoyés à la radio |
| `AccessoryManager+FromRadio.swift` | Paquets reçus de la radio |
| `AccessoryManager+Position.swift` | Partage de position GPS |
| `Gestionnaire d'accessoires+MQTT.swift` | Proxy MQTT |
| `AccessoryManager+TAK.swift` | Intégration TAK/CoT |

Les protocoles de transport sont dans `Meshtastic/Accessoire/Transports/`.
## Persistance
SwiftData est la seule couche de persistance. `PersistenceController.shared` possède le `ModelContainer`. Les vues utilisent `@Environment(\.modelContext)` ou `@Query`. Les écritures d'arrière-plan utilisent les `MeshPackets` `@ModelActor`.
Les types de modèles sont définis avec `@Model` dans `Meshtastic/Model/`. L'évolution du schéma utilise `VersionedSchema` et `SchemaMigrationPlan` dans `MeshtasticSchema.swift`.
## Services
Les services d'application qui ne sont pas liés à la connectivité radio en direct dans `Meshtastic/Services/`.
| Fichier | Responsabilité |
|------|---------------|
| `DocTranslationService.swift` | Traduction de la documentation sur l'appareil à l'aide du cadre de traduction Apple (principal) avec le repli FoundationModels. Détecte la langue de l'appareil, traduit les pages de documents en anglais groupés et gère la prélecture en arrière-plan. iOS 26+. |
| `TraductionCache.swift` | Cache basé sur des fichiers pour le contenu `.md` traduit stocké dans le support de l'application. Suit les hachages de contenu pour la détection de la sténure et applique une politique d'expulsion LRU de 50 Mo par langue. |

## Protobufs
Le paquet Swift `MeshtasticProtobufs` (`MeshtasticProtobufs/Package.swift`) enveloppe les sources Swift générées par protobuf. Régénérez avec `./scripts/gen_protos.sh` après avoir mis à jour le sous-module `protobufs/`.
## Paquets Swift externes
| Colis | But |
|---------|---------|
| [TAKPacket-SDK](https://github.com/meshtastic/TAKPacket-SDK) | Format de fil TAK V2 sur `ATAK_PLUGIN_V2 = port 78`. Expose `CotXmlParser`, `CotXmlBuilder` et `TakCompressor` (compression du dictionnaire zstd). Épinglé dans `Meshtastic.xcworkspace/... /Paquet.résolu`. Voir [TAK Protocol](tak-protocol.html). |

