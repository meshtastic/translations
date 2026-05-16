
# Wie das Meshtastic Signal Meter funktioniert
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/signalMeter_full_all_dark.png">
  <img src="../assets/screenshots/signalMeter_full_all.png" alt="Signal meter levels">
</picture>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/signalMeter_compact_all_dark.png">
  <img src="../assets/screenshots/signalMeter_compact_all.png" alt="Compact signal meter">
</picture>

Der Meshtastic-Signalmesser - oft als eine Reihe von Balken oder eine Statusfarbe in der App gesehen - wird ganz anders berechnet als die "Bars" auf einem herkömmlichen Mobiltelefon oder WLAN-Router.
Die meisten Verbrauchergeräte messen nur, wie "laut" ein Signal ist. Da Meshtastic jedoch die **LoRa (Long Range)**-Technologie verwendet, verwendet sein Signalmessgerät eine Logik, die misst, wie **klar** das Signal im Verhältnis zu den spezifischen Einstellungen ist, die Ihr Mesh verwendet.
---
## 1. Die beiden Metriken: "Laut" vs. "Klarheit"
Um das Messgerät zu verstehen, müssen Sie die beiden Messungen verstehen, die der LoRa-Radiochip jedes Mal durchführt, wenn er eine Nachricht empfängt:
* **RSSI (Received Signal Strength Indicator):** Dies ist die **Lautstärke** der Rohleistung, die auf Ihre Antenne trifft.* **SNR (Signal-zu-Rausch-Verhältnis):** Dies ist die **Klarheit** des Signals im Vergleich zum statischen Hintergrund.
> **Tip — Die Analogie:** Stellen Sie sich vor, Sie versuchen, einen Freund zu hören, der mit Ihnen spricht.> * **RSSI** ist, wie laut ihre Stimme ist.> * **The Noise Floor** ist das Hintergrundgeräusch im Raum (Klimaanlage, andere Leute sprechen, Verkehr).> * **SNR** ist, wie leicht Sie die Stimme Ihres Freundes von der Hintergrundgeräusche unterscheiden können.
Wenn dein Freund dich bei einem ohrenbetäubenden Rockkonzert anschreit, ist das Signal unglaublich laut (High RSSI), aber du kannst ihn immer noch nicht verstehen, weil das Hintergrundgeräusch lauter ist (Bad SNR). Umgekehrt, wenn Ihr Freund Ihnen in einer totstillen Bibliothek zuflüstert, ist das Signal sehr schwach (Niedrige RSSI), aber Sie können sie perfekt verstehen (Tolle SNR).
---
## 2. Die Magie von LoRa: Hören "Below the Noise Floor"
Bei Standardfunkgeräten (wie FM oder Wi-Fi) hört der Empfänger nur statisches Rauschen, wenn das Hintergrundgeräusch lauter als das Signal (ein negatives SNR) ist.
LoRa ist etwas Besonderes. Es verwendet **"Spread Spectrum"**-Modulation, die es dem Radio ermöglicht, ein Signal mathematisch aus der Luft zu ziehen, selbst wenn es tief *unter* dem Hintergrundrauschen vergraben ist. Aus diesem Grund sehen Sie häufig **negative SNR-Zahlen** in Meshtastic (z. B. -10dB, was bedeutet, dass das Signal 10 Dezibel schwächer ist als die Hintergrundstatik).
Abhängig davon, welche Meshtastic-Voreinstellung Sie verwenden (z. B.`LongFast`Vs.`ShortFast`), Hat das Radio eine bestimmte **SNR-Grenze** - die absolute maximale Menge an Lärm, die es tolerieren kann, bevor die Nachricht vollständig durch die statische Aufladung verloren geht.
---
## 3. Wie der Signalmesser die Qualität berechnet
Die Meshtastic-Apps nehmen sowohl RSSI als auch SNR und führen sie durch eine spezifische Formel aus, um Ihrem Signal eine Qualitätsbewertung zuzuweisen (Keine, Schlecht, Fair oder Gut). Es skaliert diese Werte speziell basierend auf den physischen Grenzen der von Ihnen verwendeten Funkvoreinstellung.
So entscheidet die App genau, wie viele Balken (oder welche Farbe) sie Ihnen anzeigen soll:
| Niveau | Bar | Kriterien | Bedeutung |
|-------|------|----------|---------|
| Gutes | 3 | RSSI besser als `-115 dBm` **UND** SNR über dem Basislimit für Ihre Voreinstellung | Das Signal ist sowohl laut als auch klar - gesunde Verbindung. |
| Messe | 2 | Fällt zwischen Gut und Schlecht | Das Signal wird leiser oder lauter, aber das Radio versteht die Nachricht gut. |
| Schlecht | 1 | RSSI fällt auf `-120 dBm` oder schlechter, **ODER** SNR innerhalb von `5,5 dB` des absoluten Bruchpunkts Ihrer Voreinstellung | Kaum hängend - am Rande der Reichweite oder starke Störungen. |
| Keiner | 0 | RSSI schlechter als `-126 dBm` **UND** SNR ist `7,5 dB` unter die ideale Grenze gefallen | Übertragung vollständig in statischer Vergrabung. |

---
## 4. Was das für Sie bedeutet
Da das Meshtastic-Messgerät als **"Klarheitsmesser"** fungiert, verhält es sich anders als das, was die meisten Menschen erwarten:
> **Tip — Keine Panik über niedrige RSSI:** Sie könnten einen scheinbar schrecklichen RSSI-Wert wie`-118 dBm`. Auf einem Handy hätten Sie null Balken. Aber wenn Sie eine SNR von`+2 dB`, Meshtastic wird immer noch ein starkes Signal zeigen! *Die Bibliothek ist ruhig, so dass das Flüstern perfekt zu hören ist. *
> **Warning — Achten Sie auf lokale Geräusche:** Wenn Sie eine riesige Antenne anschließen und einen großartigen RSSI sehen (z. B.`-90 dBm`) Aber Ihr Signalmesser zeigt nur **1 Bar (Schlecht)** an, Sie haben ein Problem. Das bedeutet, dass Sie lokale Störungen haben - vielleicht eine billige Stromversorgung, einen lauten Computer oder einen nahe gelegenen Funkturm - die so viel Rauschen verursachen, dass es Ihr Netz übertönt.
