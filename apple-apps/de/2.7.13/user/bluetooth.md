
# Bluetooth Device Connection

The Meshtastic app connects to your radio over Bluetooth Low Energy (BLE). You can manage multiple radios and switch between them without re-pairing.

## Connecting a Radio

1. Power on your Meshtastic radio.
2. Open the app and tap the **Connect** tab.
3. The app scans for nearby devices automatically when you are not connected.
4. Tap your device name in the list to connect.

The app remembers your preferred device and reconnects automatically when the radio is in range.

## Disconnecting a Radio

Swipe left on a connected radio in the Connect view and tap **Disconnect**. The radio continues operating on the mesh — it just stops syncing with the app.

## Live Activity

Drücken Sie lange auf eine verbundene Funkreihe, um eine Live-Aktivität zu starten (iOS 16.2+). Die Live-Aktivität zeigt den Mesh-Status auf Ihrem Sperrbildschirm und in Dynamic Island an.

## Verwaltung mehrerer Funkgeräte

Sie können mehrere Radios koppeln, aber nur eines ist auf einmal aktiv. Wechseln Sie zwischen ihnen, indem Sie in der Ansicht "Connect" auf ein anderes Gerät tippen.

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
- Stellen Sie sicher, dass Bluetooth in den iOS-Einstellungen aktiviert ist → Bluetooth.
- Bewegen Sie sich innerhalb von 10 Metern vom Radio.
- Starten Sie das Radio neu.

**Verbindung bricht wiederholt ab**
- Überprüfen Sie den Batteriestand des Radios.
- Versuchen Sie, das Gerät in den iOS-Einstellungen zu vergessen → Bluetooth und das erneute Verbinden.

**App fragt nach Bluetooth-Berechtigung**
- Erteilen Sie Berechtigungen in iOS-Einstellungen → Datenschutz & Sicherheit → Bluetooth → Meshtastic.

---

> **Tip — Angeschlossenes Radio**
> Nach links wischen, um die Verbindung zu trennen. Drücken Sie lange, um die Live-Aktivität zu starten.

