
# Nachrichten & Kanäle

Meshtastic verwendet ein Kanalsystem für Gruppenübertragungen und Direktnachrichten für private Einzelgespräche.

## Sender

### Nachrichtenverlauf

Channel-Konversationen laden standardmäßig die neuesten **50 Nachrichten**. Scrolle nach oben und tippe auf **Mehr laden**, um den nächsten Stapel abzuragen. Dadurch bleibt die App auf Kanälen mit Tausenden von Nachrichten reaktionsschnell.

### Kanalindex

| Symbol | Bedeutung |
|--------|---------|
| **0** (primärer Kreis) | Primärer Kanal - Broadcast-Pakete werden hier gesendet. Standortdaten werden vom ersten Kanal übertragen, in dem sie aktiviert sind (Firmware 2.7+). |
| **1–7** | Sekundäre Kanäle – separate Messaging-Gruppen, die jeweils durch ihren eigenen Schlüssel gesichert sind. |

### Kanalkonfiguration

![Channel form](../assets/screenshots/channelForm_primary.png)

Mit dem Kanalformular können du den Kanalnamen, den Verschlüsselungsschlüssel, die Rolle, die Positionsfreigabe und die MQTT-Uplink-/Downlink-Einstellungen konfigurieren.

### Kanalsicherheit

| Icon | Bedeutung |
|------|---------|
| ![Securely Encrypted](../assets/screenshots/lockClosed.png) | **Sicher verschlüsselt** – der Kanal verwendet einen 128-Bit- oder 256-Bit-AES-Schlüssel. |
| ![Not Securely Encrypted](../assets/screenshots/lockOpen.png) | **Nicht sicher verschlüsselt** - der Kanal verwendet keinen Schlüssel oder einen bekannten 1-Byte-Schlüssel, wird aber nicht für genaue Standortdaten verwendet. |
| ![Insecure with Location](../assets/screenshots/lockOpenRed.png) | **Unsicher mit Standort** - der Kanal ist nicht sicher verschlüsselt und wird für genaue Standortdaten verwendet. |
| ![Insecure with MQTT](../assets/screenshots/lockOpenMqtt.png) | **Unsicher mit MQTT** - nicht sicher verschlüsselt und genaue Standortdaten werden über MQTT mit dem Internet verbunden. |

---

> **Tip — Kanäle teilen**
> Ein QR-Code enthält die LoRa-Konfiguration und die Kanäle, die für die Kommunikation von Funkgeräten benötigt werden. Verwende **Kanäle ersetzen**, um zu überschreiben, oder **Kanäle hinzufügen**, um vorhandene Kanäle hinzuzufügen.

> **Tip — Kanäle verwalten**
> Der primäre Kanal verwaltet den Rundfunkverkehr. Füge sekundäre Kanäle für separate Messaging-Gruppen hinzu, die jeweils durch einen eigenen Schlüssel gesichert sind.

> **Tip — Verwaltung aktiviert**
> Wähle einen Knoten aus der Dropdown-Liste aus, um verbundene oder entfernte Geräte zu verwalten.

---

## Direktnachrichten

### Kontakte

| Element | Bedeutung |
|---------|---------|
| ![Favorites](../assets/screenshots/favorite.png) | **Favoriten** - Favoritenkontakte und Knoten mit aktuellen Nachrichten erscheinen oben in der Kontaktliste. |
| ![Long press](../assets/screenshots/longPress.png) | **Lange Druckaktionen** – langes Drücken, um den Kontakt zu favorisieren oder stumm zu schalten oder eine Unterhaltung zu löschen. |

### Verschlüsselung

![Encryption legend](../assets/screenshots/lockLegend.png)

| Icon | Bedeutung |
|------|---------|
| ![Shared Key](../assets/screenshots/lockOpen.png) | **Geteilter Schlüssel** – Direktnachrichten verwenden den freigegebenen Schlüssel für den Kanal. |
| ![Public Key Encryption](../assets/screenshots/lockClosed.png) | **Verschlüsselung mit öffentlichen Schlüsseln** - Direktnachrichten verwenden die Infrastruktur für öffentliche Schlüssel zur Verschlüsselung. Erfordert Firmware 2.5 oder höher. |
| ![PKI Mismatch](../assets/screenshots/keySlash.png) | **Public Key Mismatch** - der neueste öffentliche Schlüssel für diesen Knoten stimmt nicht mit dem zuvor aufgezeichneten Schlüssel überein. Überprüfe, mit wem du Nachrichten senden, indem du öffentliche Schlüssel persönlich oder telefonisch vergleichen. |

---

### Tapback-Reaktionen

Drücke lange auf eine beliebige Nachricht und tippe auf **Tapback**, um eine Emoji-Reaktion zu senden.

![Tapback input](../assets/screenshots/tapbackInput.png)

---

> **Tip — Nachrichten**
> Sende Kanalsendungen und Direktnachrichten. Drücke lange auf eine beliebige Nachricht für Aktionen wie Kopieren, Antworten, Tapback und Lieferdetails.

---

## Nachrichtenstatus

![Message status reference](../assets/screenshots/ackErrors.png)

| Farbe | Bedeutung |
|--------|---------|
| Grau | Erfolgreiche Lieferung. |
| Orangenblase | **Bekannt von einem anderen Knoten** - Nachricht wurde weitergeleitet, aber nicht vom endgültigen Empfänger bestätigt. |

Die folgenden Fehler können in einer Sprechblase angezeigt werden (rot, sofern nicht anders angegeben):

| Status | Beschreibung |
|--------|-------------|
| Keine Route | Es wurde keine Route zum Zielknoten gefunden. |
| Habe NAK | Der Zielknoten hat die Nachricht ausdrücklich abgelehnt. |
| Auszeit | Die Nachricht hat eine Zeit überschreitet und wartet auf die Bestätigung. |
| Keine Schnittstelle | Die Funkschnittstelle ist nicht verfügbar. |
| Maximale Übertragung | Maximale Wiederholungsversuche ohne Erfolg erreicht. |
| Kein Kanal | Der angegebene Kanal existiert auf dem Ziel nicht. |
| Zu groß | Das Paket überschreitet die maximal zulässige Größe. |
| Keine Antwort | Keine Antwort vom Ziel erhalten. |
| PKI fehlgeschlagen | Verschlüsselung/Entschlüsselung der öffentlichen Schlüsselinfrastruktur fehlgeschlagen. |
| Schlechte Anfrage | Fehlgebildetes Paket vom Ziel abgelehnt. |
| Nicht autorisiert | Der Zielknoten hat die Anfrage aufgrund von Berechtigungen abgelehnt. |

> Grau zeigt eine erfolgreiche Lieferung an. Orange zeigt einen erneuten Fehler an. Rot zeigt einen dauerhaften Fehler an, der bei einem erneuten Versuch nicht erfolgreich sein wird.

---

## Link-Darstellung

Links in Nachrichtenblasen - einschließlich URLs, Meshtastic-Kanallinks und Markdown`[text](url)`Links - sind mit einer Unterstinde und der Designstandards Link-Farbe (Blau 400) gestaltet. Dadurch unterscheiden sich Links sowohl im hellen als auch im dunklen Modus visuell von normalem Nachrichtentext. Das Tippen auf einen Link wird im Browser geöffnet, oder für Meshtastic-Kanal-/Kontakt-URLs wird der entsprechende In-App-Handler geöffnet.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/messageText_link_dark.png">
  <img src="../assets/screenshots/messageText_link.png" alt="Message bubble with styled link">
</picture>

---

## Nachrichtenformatierung (iOS 18+)

Unter iOS 18 und neuer werden Formatierungsschaltflächen in der kompakten Symbolleiste unter dem Feld zum Verfassen angezeigt, nachdem du mindestens 3 Zeichen eingegeben haben. Die Formatierungsschaltflächen teilen sich die Symbolleistenzeile mit der Alarmglocke, dem Positionsstift und dem Bytezähler – alles als kompakte Symbole. Die Symbolleiste scrollt horizontal, wenn sie die Bildschirmbreite überschreitet.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/composeArea_formatting_dark.png">
  <img src="../assets/screenshots/composeArea_formatting.png" alt="Compose area with formatting toolbar and live preview">
</picture>

### Unterstützte Stile

| Knopf | Style | Markdown-Syntax |
|--------|-------|-----------------|
| Fettgedrucktes SF-Symbol | Fett | `**Text**` |
| Ikritisches SF-Symbol | Kursivschrift | `*Text*` |
| Durchstreches SF-Symbol | Durchstren | `~~Text~~` |
| Code SF-Symbol | Code | `` `Text` `` |
| SF-Symbol verknüpfen | Link | `[Text](url)` |

### Wie man Text formatiert

1. **Wähle Text aus und tippe auf eine Schaltfläche** - Wähle ein Wort oder einen Satz im Feld "Komponieren" aus, und tippe dann auf eine Schaltfläche "Formatierung". Die entsprechenden Markdown-Trennzeichen werden um die Auswahl herum eingefügt. Alle vorhandenen Markdown-Trennzeichen innerhalb der Auswahl werden zuerst entfernt, um eine überlappende Syntax zu vermeiden. Leerzeichen an den Rändern der Auswahl werden außerhalb der Trennzeichen verschoben, so dass Markdown korrekt gerendert wird.
2. **Tippe zuerst auf eine Schaltfläche und gib dann ein** – tippe mit dem Cursor (keine Auswahl) auf eine Formatierungsschaltfläche. Trennzeichen werden eingefügt und der Cursor wird zwischen ihnen platziert, so dass du sofort formatierten Text eingeben können.
3. **Ausschalten** - Wähle Text aus, der bereits mit Trennzeichen umwickelt ist, und tippe auf dieselbe Formatierungsschaltfläche, um die Trennzeichen zu entfernen.

### Live-Vorschau

Wenn das Feld Compose Markdown-Syntax enthält, wird eine Vorschaublase über dem Compose-Feld angezeigt, die zeigt, wie die Nachricht beim Senden aussieht. Die Vorschau wird in Echtzeit aktualisiert, während du tippen. Wenn kein Markdown vorhanden ist, wird die Vorschau ausgeblendet.

Die Markdown-Formatierung wird auch in den Vorschauen der Kanal- und Benutzernachrichtenliste gerendert, so dass du formatierten Text auf einen Blick sehen können.

| Beispiel | Beschreibung |
|---------|-------------|
| ![Bold preview](../assets/screenshots/messagePreview_bold.png) | Vorschau, die die **fett**-Formatierung zeigt, die auf Text angewendet wird. |
| ![Mixed preview](../assets/screenshots/messagePreview_mixed.png) | Vorschau zeigt **fett**, *kursiv*, ~~strikethrough~~ und `code`-Formatierung kombiniert. |

### Stile wechseln

Wenn du Text auswählen, der bereits Markdown-Trennzeichen enthält, und einen anderen Stil anwenden, werden die vorhandenen Trennzeichen entfernt und durch den neuen Stil ersetzt. Zum Beispiel die Auswahl von`**bold**`Und Tippen Strikethrough produziert`~~bold~~`.

Nach dem Anwenden eines Stils wird die Auswahl erweitert, um die Trennzeichen einzuschließen (z. B. Auswahl`dolphin`Und tippe auf "Fett"`**dolphin**`), Wodurch es einfach ist, sofort auszuschalten oder zu einem anderen Stil zu wechseln.

### Auswahlsicherheit

Wenn deine Auswahl vorhandene Trennzeichen teilweise überschneidet, wird die Auswahl automatisch erweitert, um den vollständigen Trennzeichen vor der Formatierung einzuschließen. Alle verwaisten (ungepaarten) Trennzeichen, die an anderer Stelle im Text verbleiben, werden automatisch bereinigt. Dies verhindert verstümmlte Abschläge wie`th***~~~~~~e~~`.

### iOS 17 Benutzer

Die Formatierungssymbolleiste ist nur für iOS 18 und neuer verfügbar. Benutzer mit iOS 17.x sehen das Standard-Komponierfeld ohne Änderungen an ihrer Erfahrung.

### Mac Catalyst

Auf dem Mac Catalyst sendet das Drücken von **Enter** die Meldung. Drücke **Umschalt+Enter**, um einen Zeilenüberbruch einzufügen. Die Schaltfläche der Zeichenpalette bleibt neben den Formatierungsschaltflächen verfügbar.

> **Tip — Nachrichtenlimit**
> Nachrichten sind auf 200 Byte begrenzt. Markdown-Trennzeichen zählen zu diesem Limit (z.B.`**bold**`Verwendet 4 zusätzliche Bytes für die`**`Paare). Der Bytezähler in der Symbolleiste zeigt den verbleibenden Speicherplatz an.

