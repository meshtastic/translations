
# Découverte de maillage local

Local Mesh Discovery analyse votre région pour trouver des radios Meshtastic à proximité fonctionnant sur différents paramètres de fréquence. Utilisez-le pour identifier la configuration LoRa la plus active dans votre emplacement avant de configurer un nouveau nœud.

## Ce qu'il fait

Le scanner passe par une série de préréglages de modem LoRa et d'emplacements de fréquence, écoute une période définie sur chacun d'entre eux et enregistre le nombre de nœuds qu'il entend et l'occupation des ondes (utilisation des canaux). Il présente ensuite une liste classée des paramètres classés par activité.

Sur les appareils pris en charge exécutant iOS 26+, l'assistant d'IA sur l'appareil analyse les résultats de l'analyse et recommande la meilleure configuration pour votre emplacement - aucune connexion Internet n'est requise.

## Exécution d'un scan

![Radar sweep active — scan in progress](../assets/screenshots/radarActive.png)

1. Allez dans **Paramètres → Découverte locale du maillage**.
2. Appuyez sur **Démarrer l'analyse**.
3. Le scanner passe automatiquement par les paramètres. Chaque cycle prend quelques minutes - ne fermez pas l'application pendant une analyse.
4. Une fois l'analyse terminée, les résultats apparaissent classés par nombre de nœuds et par activité des canaux.

## Résultats de lecture

![Discovery results summary with two presets](../assets/screenshots/summaryTwoPresets.png)

| Colonne | Description |
|--------|-------------|
| Programmer | Préréglage du modem LoRa (par exemple, Long Rapide, Long Lent) |
| Noeuds Entendus | Nombre de nœuds distincts détectés sur ce paramètre |
| Utilisation du canal | Pourcentage de temps d'antenne utilisé - plus élevé signifie plus actif |
| Recommandation | ✅ Meilleur match pour votre région |

## Appliquer un paramètre

Appuyez sur une ligne de résultat, puis sur **Appliquer le paramètre** pour configurer votre radio connectée pour qu'elle corresponde au paramètre le plus actif de votre région. Cela met directement à jour la configuration LoRa sur la radio.

---

> **Tip — Qu'est-ce que ça fait ?**
> Cet outil analyse votre zone locale pour trouver des radios Meshtastic à proximité sur différents paramètres de fréquence. Il bascule automatiquement entre les paramètres, écoute pendant quelques minutes sur chacun d'eux, puis vous montre quel paramètre fonctionne le mieux pour votre emplacement en fonction du nombre de radios qu'il trouve et de l'activité des ondes. Sur les appareils pris en charge, l'IA locale sur l'appareil analysera les résultats de votre analyse et vous recommandera le meilleur paramètre - aucune connexion Internet n'est requise.

