
# Lokale Mesh-Entdeckung

Local Mesh Discovery durchsucht deinen Bereich, um Meshtastic-Funkgeräte in der Nähe zu finden, die mit verschiedenen Frequenzeinstellungen arbeiten. Verwenden du es, um zu ermitteln, welche LoRa-Konfiguration an deinem Standort am aktivsten ist, bevor du einen neuen Knoten einrichten.

## Was Es Tut

Der Scanner wechselt durch eine Reihe von LoRa-Modem-Voreinstellungen und Frequenzschlitzen, hört sich einen bestimmten Zeitraum an und zeichnet auf, wie viele Knoten er hört und wie beschäftigt die Ätherwellen sind (Kanalnutzung). Anschließend wird eine Rangliste der Einstellungen nach Aktivität angezeigt.

Auf unterstützten Geräten mit iOS 26+ analysiert der KI-Assistent auf dem Gerät die Scanergebnisse und empfiehlt die beste Konfiguration für deinen Standort - keine Internetverbindung erforderlich.

## Einen Scan ausführen

![Radar sweep active — scan in progress](../assets/screenshots/radarActive.png)

1. Gehen du zu **Einstellungen → Lokale Mesh-Entdeckung**.
2. Tippen du auf **Scan starten**.
3. Der Scanner wechselt automatisch durch die Einstellungen. Jeder Zyklus dauert einige Minuten – schließen du die App nicht während eines Scans.
4. Wenn der Scan abgeschlossen ist, werden die Ergebnisse nach der Anzahl der Knoten und der Kanalaktivität sortiert angezeigt.

## Ergebnisse lesen

![Discovery results summary with two presets](../assets/screenshots/summaryTwoPresets.png)

| Spalte | Beschreibung |
|--------|-------------|
| Vorher einstellen | LoRa-Modem-Voreinstellung (z. B. Long Fast, Long Slow) |
| Knoten Gehört | Anzahl der unterschiedlichen Knoten, die in dieser Einstellung erkannt wurden |
| Kanalnutzung | Prozentsatz der verbrauchten Sendezeit - höher bedeutet aktiver |
| Empfehlung | ✅ Beste Übereinstimmung für deine Region |

## Eine Einstellung anwenden

Tippen du auf eine Ergebniszeile und dann auf **Einstellung anwenden**, um dein angeschlossenes Radio so zu konfigurieren, dass es der aktivsten Einstellung in deinem Bereich entspricht. Dadurch wird die LoRa-Konfiguration direkt im Radio aktualisiert.

---

> **Tip — Was macht das? **
> Dieses Werkzeug scannt deine Umgebung, um Meshtastic-Funkgeräte in der Nähe mit verschiedenen Frequenzeinstellungen zu finden. Es wechselt automatisch zwischen den Einstellungen, hört sich jeweils einige Minuten lang ab und zeigt dir dann an, welche Einstellung für deinen Standort am besten funktioniert, basierend auf der Anzahl der Radios, die es findet und wie beschäftigt die Äther sind. Auf unterstützten Geräten analysiert die lokale KI auf dem Gerät deine Scanergebnisse und empfiehlt die beste Einstellung - keine Internetverbindung erforderlich.

