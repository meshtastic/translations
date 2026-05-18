
# Aperçu de l'architecture

L'application Meshtastic Apple cible iOS, iPadOS et macOS (via Mac Catalyst). Il communique avec les radios Meshtastic sur BLE, TCP/IP et (sur macOS) en série.

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
| `AccessoryManager+MQTT.swift` | Proxy MQTT |
| `AccessoryManager+TAK.swift` | Intégration TAK/CoT |

Les protocoles de transport sont en`Meshtastic/Accessory/Transports/`.

## Persistance

SwiftData est la seule couche de persistance.`PersistenceController.shared`Possède le`ModelContainer`. Les vues utilisent`@Environment(\.modelContext)`Ou`@Query`. Les écritures d'arrière-plan utilisent le`MeshPackets` `@ModelActor`.

Les types de modèles sont définis avec`@Model`Dans`Meshtastic/Model/`. Utilisations de l'évolution du schéma`VersionedSchema`Et`SchemaMigrationPlan`Dans`MeshtasticSchema.swift`.

## Services

Les services d'application qui ne sont pas liés à la connectivité radio en direct`Meshtastic/Services/`.

| Fichier | Responsabilité |
|------|---------------|
| `DocTranslationService.swift` | Traduction de la documentation sur l'appareil à l'aide du cadre de traduction Apple (principal) avec le repli FoundationModels. Traduit les fichiers sources de démarquement anglais groupés, les caches sont traduits`.md`, Se convertit en HTML via`MarkdownConverter`, Et déclenche le téléchargement automatique après la prélecture. iOS 26+. |
| `TranslationCache.swift` | Cache basé sur des fichiers pour traduit`.md`Contenu stocké dans le support d'application. Suit les hachages de contenu pour la détection de la sténure et applique une politique d'expulsion LRU de 50 Mo par langue. |
| `MarkdownConverter.swift` | Markdown→Convertisseur HTML compatible GFM. Prend en charge les titres, les paragraphes, les listes, les clôtures de code, le code en ligne, les tableaux, les liens, les images, le passage HTML (`<picture>`,`<img>`), Appels de guillemets en bloc (conseil/avertissement), gras, italique, barré, règles horizontales, et`.md`→`.html`Réécriture du lien. Dénude la matière avant YAML et les attributs en ligne Jekyll. |
| `DocsTranslationUploader.swift` | Commit automatiquement traduit`.md`Fichiers à`meshtastic/translations`Repo après la fin de la prélecture de l'arrière-plan. Effectue des vérifications en lecture seule par rapport à`meshtastic/meshtastic`Et`meshtastic/translations`(Pas d'authentification), puis s'engage via l'API de contenu GitHub en utilisant un PAT à grain fin de`Secrets.json`. Le suivi par fichier permet de réessayer les téléchargements ayant échoué. |
| `CommunityTranslationFetcher.swift` | Télécharge les traductions de la communauté existantes à partir du flux CDN GitHub Pages (`index.json`) Avant de revenir à la traduction sur l'appareil. Ferrait`nav-labels.json`Et`search-index.json`Pour les chaînes d'interface utilisateur traduites et les mots-clés de recherche. Construit un dossier traduit pré-rendu afin que`DocBundle`Peut charger des pages traduites directement. |

## Protobufs

Le`MeshtasticProtobufs`Paquet Swift (`MeshtasticProtobufs/Package.swift`) Enveloppe les sources Swift générées par protobuf. Régénérer avec`./scripts/gen_protos.sh`Après avoir mis à jour le`protobufs/`Sous-module.

