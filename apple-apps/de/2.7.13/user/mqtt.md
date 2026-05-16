
# MQTT

Das MQTT-Modul ermöglicht es einem Meshtastic-Knoten, den Mesh-Verkehr mit einem MQTT-Broker zu verbinden, das Mesh über das Internet zu erweitern und die Integration mit Hausautomationssystemen zu ermöglichen.

## Was MQTT macht

Ein Knoten mit aktiviertem MQTT fungiert als Gateway: Er veröffentlicht empfangene Mesh-Pakete an einen MQTT-Broker und abonniert optional ein Thema, so dass Remote-Knoten Pakete wieder in das lokale Mesh einspeisen können.

| Icon | Staat | Beschreibung |
|------|-------|-------------|
| ![MQTT connected](../assets/screenshots/mqttConnected.png) | Verwandt | Die MQTT-Bridge ist aktiv - Uplink und Downlink sind beide aktiviert. |
| ![MQTT uplink only](../assets/screenshots/mqttUplinkOnly.png) | Nur Uplink | Mesh-Pakete an den Broker veröffentlichen, aber eingehende Pakete nicht abonnieren. |
| ![MQTT disconnected](../assets/screenshots/mqttDisconnected.png) | Abgetrennt | MQTT ist konfiguriert, aber derzeit nicht mit dem Broker verbunden. |

Dadurch können zwei Mesh-Netzwerke an verschiedenen physischen Standorten als ein logisches Netzwerk angezeigt werden - solange mindestens ein Knoten an jedem Standort über einen Internetzugang verfügt.

## Konfiguration von MQTT

Gehen Sie zu **Einstellungen → MQTT**:

![MQTT Config](../assets/screenshots/mqttConfig.png)

| Kulisse | Beschreibung |
|---------|-------------|
| MQTT-Server | Hostname oder IP Ihres MQTT-Brokers (z. B. `mqtt.meshtastic.org` für den öffentlichen Broker). |
| Hafen | Der Standardwert ist 1883 (unverschlüsselt) oder 8883 (TLS). |
| Benutzername | MQTT-Broker-Benutzername (optional). |
| Passwort | MQTT-Broker-Passwort (optional). |
| Wurzelthema | Das Themenpräfix für alle veröffentlichten Nachrichten (Standard: `msh`). |
| Aktiviert | Schalten Sie die MQTT-Brückung ein/aus. |
| Verschlüsselung aktiviert | Verschlüsseln Sie Pakete vor der Veröffentlichung. Empfohlen - verhindert, dass der Broker den Inhalt der Nachricht liest. |
| JSON aktiviert | Veröffentlichen Sie dekodierte JSON-Pakete zusätzlich zum binären Protobuf-Format. Nützlich für Hausautomationsintegrationen. |
| TLS aktiviert | Verwenden Sie TLS für die MQTT-Verbindung. Erfordert einen Broker mit TLS-Unterstützung. |
| Proxy an den Kunden | Leiten Sie den MQTT-Verkehr über die Telefon-App und nicht direkt über das Radio weiter. Nützlich für Radios ohne WLAN. |

## Themenstruktur

Meshtastic veröffentlicht an:

```
<root_topic>/<region>/<channel_index>/<node_id>/<packet_type>
```

Beispiel:`msh/US/2/!a1b2c3d4/text`

## Sicherheitsüberlegungen

- Aktivieren von MQTT mit einem unsicheren Kanal sendet Standort und Nachrichten an das Internet.
- Die Kanalsicherheitsanzeige zeigt **Unsicher mit MQTT** (🔓⚠️) an, wenn ein Kanal nicht verschlüsselt und MQTT aktiv ist.
- Verwenden Sie immer **Verschlüsselung aktiviert** in der Produktion, um den Inhalt der Nachricht zu schützen.
- Erwägen Sie, einen privaten Makler anstelle des öffentlichen zu verwenden`mqtt.meshtastic.org`.

## Öffentlicher Makler

Der öffentliche MQTT-Broker bei`mqtt.meshtastic.org`Steht zum Testen zur Verfügung. **Übermitteln Sie keine sensiblen Informationen über den öffentlichen Makler. ** Verwenden Sie es nur für die erste Einrichtungsprüfung.

