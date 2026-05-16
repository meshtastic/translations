
# Bluetooth-Geräteverbindung

Die Meshtastic-App verbindet sich über Bluetooth Low Energy (BLE) mit deinem Radio. du können mehrere Funkgeräte verwalten und zwischen ihnen wechseln, ohne sie erneut zu koppeln.

## Anschluss eines Funkgeräts

1. Schalten du dein Meshtastic-Radio ein.
2. Öffnen du die App und tippen du auf die Registerkarte **Verbinden**.
3. Die App scannt automatisch nach Geräten in der Nähe, wenn du nicht verbunden sind.
4. Tippen du auf deinen Gerätenamen in der Liste, um eine Verbindung herzustellen.

Die App merkt sich dein bevorzugtes Gerät und verbindet sich automatisch wieder, wenn das Radio in Reichweite ist.

## Trennen eines Funkgeräts

Wischen du in einem verbundenen Radio in der Ansicht "Verbinden" nach links und tippen du auf **Trennen**. Das Radio funktioniert weiterhin auf dem Netz - es hört einfach auf, mit der App zu synchronisieren.

## Live-Aktivität

Drücken du lange auf eine verbundene Funkreihe, um eine Live-Aktivität zu starten (iOS 16.2+). Die Live-Aktivität zeigt den Mesh-Status auf deinem Sperrbildschirm und in Dynamic Island an.

## Verwaltung mehrerer Funkgeräte

du können mehrere Radios koppeln, aber nur eines ist auf einmal aktiv. Wechseln du zwischen ihnen, indem du in der Ansicht "Connect" auf ein anderes Gerät tippen.

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
- Stellen du sicher, dass Bluetooth in den iOS-Einstellungen aktiviert ist → Bluetooth.
- Bewegen du sich innerhalb von 10 Metern vom Radio.
- Starten du das Radio neu.

**Verbindung bricht wiederholt ab**
- Überprüfen du den Batteriestand des Radios.
- Versuchen du, das Gerät in den iOS-Einstellungen zu vergessen → Bluetooth und das erneute Verbinden.

**App fragt nach Bluetooth-Berechtigung**
- Erteilen du Berechtigungen in iOS-Einstellungen → Datenschutz & Sicherheit → Bluetooth → Meshtastic.

---

> **Tip — Angeschlossenes Radio**
> Nach links wischen, um die Verbindung zu trennen. Drücken du lange, um die Live-Aktivität zu starten.

