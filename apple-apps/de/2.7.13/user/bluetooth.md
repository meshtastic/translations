
# Bluetooth-Geräteverbindung

Die Meshtastic-App verbindet sich über Bluetooth Low Energy (BLE) mit deinem Radio. du können mehrere Funkgeräte verwalten und zwischen ihnen wechseln, ohne sie erneut zu koppeln.

## Anschluss eines Funkgeräts

1. Schalte dein Meshtastic-Radio ein.
2. Öffne die App und tippe auf die Registerkarte **Verbinden**.
3. Die App scannt automatisch nach Geräten in der Nähe, wenn du nicht verbunden sind.
4. Tippe auf deinen Gerätenamen in der Liste, um eine Verbindung herzustellen.

Die App merkt sich dein bevorzugtes Gerät und verbindet sich automatisch wieder, wenn das Radio in Reichweite ist.

## Trennen eines Funkgeräts

Wische in einem verbundenen Radio in der Ansicht "Verbinden" nach links und tippe auf **Trennen**. Das Radio funktioniert weiterhin auf dem Netz - es hört einfach auf, mit der App zu synchronisieren.

## Live-Aktivität

Drücke lange auf eine verbundene Funkreihe, um eine Live-Aktivität zu starten (iOS 16.2+). Die Live-Aktivität zeigt den Mesh-Status auf deinem Sperrbildschirm und in Dynamic Island an.

## Verwaltung mehrerer Funkgeräte

du können mehrere Radios koppeln, aber nur eines ist auf einmal aktiv. Wechsle zwischen ihnen, indem du in der Ansicht "Connect" auf ein anderes Gerät tippen.

## BLE-Signalstärke

Die App zeigt die Bluetooth-Signalstärke von Geräten in der Nähe während des Scannens an:

![BLE Signal Strength](../assets/screenshots/bleSignalStrength.png)

## Verbindungsstatus-Symbole

| Icon | Bedeutung |
|------|---------|
| ![BLE connected](../assets/screenshots/btConnected.png) | Verbunden über BLE |
| ![Reconnecting](../assets/screenshots/btReconnecting.png) | Erneute Verbindung / erneuter Versuch |
| ![TCP connected](../assets/screenshots/tcpConnected.png) | Verbunden über TCP/IP |
| ![Serial connected](../assets/screenshots/serialConnected.png) | Verbunden über serielle (macOS) |

## Vermittlung

**Radio erscheint nicht in der Liste**
- Stelle sicher, dass Bluetooth in den iOS-Einstellungen aktiviert ist → Bluetooth.
- Bewegen du sich innerhalb von 10 Metern vom Radio.
- Starte das Radio neu.

**Verbindung bricht wiederholt ab**
- Überprüfe den Batteriestand des Radios.
- Versuche, das Gerät in den iOS-Einstellungen zu vergessen → Bluetooth und das erneute Verbinden.

**App fragt nach Bluetooth-Berechtigung**
- Erteile Berechtigungen in iOS-Einstellungen → Datenschutz & Sicherheit → Bluetooth → Meshtastic.

---

> **Tip — Angeschlossenes Radio**
> Nach links wischen, um die Verbindung zu trennen. Drücke lange, um die Live-Aktivität zu starten.

