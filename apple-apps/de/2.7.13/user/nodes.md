
# Knotenliste
Der Tab "Nodes" zeigt jedes Gerät an, das Ihr Funkgerät auf dem Netz gehört hat. Tippen Sie auf einen beliebigen Knoten, um Details zu erhalten.
## Knotenstatus
| Element | Bedeutung |
|---------|---------|
| ![Node circle](../assets/screenshots/circleTextDefault.png) | **Kurzer Name & Langer Name** - jeder Knoten hat einen kurzen Namen (bis zu 4 Bytes), der im farbigen Kreis angezeigt wird, und einen langen Namen, der daneben angezeigt wird. Die Kreisfarbe wird von der Knotennummer abgeleitet. Der kurze Name kann ein Emoji oder Initialen sein. |
| ![Online](../assets/screenshots/nodeOnline.png) | **Online** - der Knoten wurde kürzlich gehört und gilt als online. |
| ![Idle / Sleeping](../assets/screenshots/nodeIdle.png) | **Leerlauf / Schlafend** - der Knoten wurde in letzter Zeit nicht gehört und ist möglicherweise in Ruhe oder außerhalb der Reichweite. |
| ![Hops Away](../assets/screenshots/hopsAway.png) | **Hops Away** - die Anzahl der Zwischenknoten, die Nachrichten zwischen Ihnen und diesem Knoten weiterleiten. Kein Hopfen bedeutet direkte Kommunikation. |

## Verschlüsselung
| Icon | Bedeutung |
|------|---------|
| ![Shared Key](../assets/screenshots/lockOpen.png) | **Geteilter Schlüssel** – Direktnachrichten verwenden den freigegebenen Schlüssel für den Kanal. |
| ![Public Key Encryption](../assets/screenshots/lockClosed.png) | **Verschlüsselung mit öffentlichen Schlüsseln** - Direktnachrichten verwenden eine Infrastruktur für öffentliche Schlüssel. Erfordert Firmware 2.5+. |
| ![PKI Mismatch](../assets/screenshots/keySlash.png) | **Public Key Mismatch** – Public Key stimmt nicht mit dem zuvor aufgezeichneten Schlüssel überein. Überprüfen Sie den Kontakt außerhalb des Bands. |

## Geräterollen
Jeder Knoten ist mit einer Rolle konfiguriert, die bestimmt, wie er sich auf dem Netz verhält. Rollen werden in der Knotendetailansicht angezeigt.
| Icon | Rolle | Beschreibung |
|------|------|-------------|
| ![](../assets/screenshots/roleClient.png) | Kunde | Standard-Endbenutzer-Gerät. Sendet und empfängt Nachrichten, teilt Position. |
| ![](../assets/screenshots/roleClientMute.png) | Kunde stummgeschaltet | Wie Client, leitet aber keine Pakete von anderen Geräten weiter. Reduziert den Netzverkehr in der Nähe von überlasteten Gebieten. |
| ![](../assets/screenshots/roleClientHidden.png) | Kunde versteckt | Sendet nur nach Bedarf für Stealth- oder Energieeinsparungen. |
| ![](../assets/screenshots/roleClientBase.png) | Kundenstamm | Dachknoten, der Nachrichten von nahe gelegenen Client Mute-Knoten verbreitet. |
| ![](../assets/screenshots/roleRouter.png) | Router | Dedizierter Infrastrukturknoten - übertreibt Pakete immer einmal neu. Am besten an strategischen Standorten positioniert, um die Abdeckung zu maximieren. |
| ![](../assets/screenshots/roleRouterLate.png) | Router Zu Spät | Wie Router, aber einmal nach allen anderen Knoten erneut übertragen. Besser geeignet für Dacheinsätze. |
| ![](../assets/screenshots/roleTracker.png) | Spürhund | Überträgt GPS-Positionspakete als Priorität. Optimiert für häufige Standortberichte. |
| ![](../assets/screenshots/roleSensor.png) | Sensor | Überträgt Telemetriepakete als Priorität. Optimiert für Sensordaten. |
| ![](../assets/screenshots/roleTak.png) | TAK | Optimiert für die ATAK-Systemkommunikation. Reduziert routinemäßige Sendungen. |
| ![](../assets/screenshots/roleTakTracker.png) | TAK-Tracker | Aktiviert automatische TAK PLI-Sendungen. Reduziert routinemäßige Sendungen. |
| ![](../assets/screenshots/roleLostAndFound.png) | Fundbüro | Überträgt den Standort als Nachricht an den Standardkanal, um die Gerätewiederherstellung zu unterstützen. |

[Auswahl der richtigen Geräterolle →](https://meshtastic.org/blog/choosing-the-right-device-role/)
## Vollständige Beispiele für Knotenzeilen
Die vollständige Knotenzeile zeigt den Kreis-Avatar, den Batteriestand, den Verschlüsselungsstatus, die zuletzt gehörte Zeit, die Geräterolle, die Signalstärke und die Protokollanzeigen auf einmal an.
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/standard_directConnected_dark.png" />
  <img src="../assets/screenshots/standard_directConnected.png" alt="Directly connected node, favorite, with signal meter" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/standard_multiHop_dark.png" />
  <img src="../assets/screenshots/standard_multiHop.png" alt="Multi-hop node 4 hops away" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/standard_mqtt_dark.png" />
  <img src="../assets/screenshots/standard_mqtt.png" alt="MQTT-bridged node" />
</picture>

## Beispiele Für Kompakte Knotenzeilen
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_directConnected_allInfo_dark.png" />
  <img src="../assets/screenshots/compact_directConnected_allInfo.png" alt="Directly connected node with all telemetry info" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_multiHop_dark.png" />
  <img src="../assets/screenshots/compact_multiHop.png" alt="Multi-hop node 7 hops away" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_withPosition_dark.png" />
  <img src="../assets/screenshots/compact_withPosition.png" alt="Node with position, 1 hop" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_pkiMismatch_dark.png" />
  <img src="../assets/screenshots/compact_pkiMismatch.png" alt="PKI key mismatch node" />
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/compact_mqtt_dark.png" />
  <img src="../assets/screenshots/compact_mqtt.png" alt="MQTT-bridged node" />
</picture>

## Zusätzliche Symbole
Tippen Sie auf einen Knoten und scrollen Sie zum Abschnitt "Protokolle", um detaillierte Metriken zu erhalten:
| Protokoll | Beschreibung |
|-----|-------------|
| ![Distance & Bearing](../assets/screenshots/logDistance.png) | Richtung und Entfernung zum Knoten basierend auf GPS. Erfordert beide Geräte, um den Standort zu teilen. |
| ![Channel badge](../assets/screenshots/channelBadge.png) | Der nummerierte Kreis zeigt an, welchen Kanal der Knoten verwendet. Nur für sekundäre Kanäle angezeigt (nicht primärer Kanal 0). |
| ![Device Metrics](../assets/screenshots/logDeviceMetrics.png) | Batteriestand, Spannung, Kanalauslastung und vom Knoten gemeldete Sendezeit. |
| ![Positions](../assets/screenshots/logPositions.png) | GPS-Positionsdaten einschließlich Breitengrad, Längengrad und Höhe. |
| ![Environment](../assets/screenshots/logEnvironment.png) | Sensordaten: Temperatur, Luftfeuchtigkeit, Luftdruck. |
| ![Detection Sensor](../assets/screenshots/logDetectionSensor.png) | Bewegungs- oder Türöffnungs-/Schließwarnungen vom Knoten. |
| ![Trace Routes](../assets/screenshots/logTraceRoutes.png) | Aufgezeichnete Trace-Routenpfade, die die Hüpfen zeigen, die eine Nachricht durch das Netz genommen hat. |

## Knotendetailansicht
Tippen Sie auf einen beliebigen Knoten, um die vollständige Detailansicht mit Hardwareinformationen, Signalmetriken, Umgebungssensoren und Protokollnavigation anzuzeigen:
![Node Detail](../assets/screenshots/nodeDetail.png)

[Dokumente zur Gerätekonfiguration →](https://meshtastic.org/docs/configuration/radio/device/)
