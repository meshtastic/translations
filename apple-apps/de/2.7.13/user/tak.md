
# TAK-Integration
Die Meshtastic-App unterstützt die Integration von Team Awareness Kit (TAK) und ermöglicht die Interoperabilität mit ATAK (Android Team Awareness Kit), iTAK und anderen CoT-kompatiblen Systemen (Cursor-on-Target) über LoRa-Mesh-Funkgeräte - kein Mobilfunk oder Internet erforderlich.
## Was ist TAK?
TAK ist eine Plattform für das Situationsbewusstsein, die häufig in taktischen Kontexten, im Notfallmanagement und in der Freizeitgestaltung im Freien verwendet wird. Es zeigt die Positionen und den Status der Teammitglieder auf einer freigegebenen Karte an. Meshtastic verbindet TAK-Benutzer mit LoRa, sodass Teams in Verbindung bleiben, ohne Mobilfunk- oder Internetabdeckung zu benötigen.
## Unterstützte Geräterollen
Die TAK-Integration funktioniert mit zwei Geräterollen:
| Icon | Rolle | Beschreibung |
|------|------|-------------|
| ![TAK](../assets/screenshots/roleTak.png) | TAK | Vollständige TAK-Rolle - sendet CoT-Positionsberichte und kann TAK-Datenpakete weiterleiten. |
| ![TAK Tracker](../assets/screenshots/roleTakTracker.png) | TAK-Tracker | Leichte TAK-Rolle nur für Positionen. Geringerer Stromverbrauch, kein Paketrelais. |

Stellen Sie die Geräterolle in **Einstellungen → Gerät** ein.
> **Tip — Firmware-Version**> Das vollständige TAK V2-Drahtformat (Formen, Routen, Marker, Casevac, Notfall) erfordert Firmware **2.8.0 oder höher** auf dem angeschlossenen Funkgerät. Ältere Firmware unterstützt immer noch PLI und GeoChat über das ältere V1-Format - die App fällt automatisch zurück.
## TAK-Server-Bildschirm
**Einstellungen → TAK Server** ist das einzige Ziel für alles, was mit TAK zu tun hat. Der Bildschirm ist von oben nach unten organisiert, so dass Sie Ihre Identität konfigurieren, den Server starten und einen ATAK / iTAK-Client in einem Durchgang koppeln können.
### TAK Identität
Der erste Abschnitt, **TAK Identity**, steuert das Team auf Firmware-Ebene und die Rollenidentität, die das Radio jedem Positionsbericht zufügt:
| Kulisse | Beschreibung |
|---------|-------------|
| Mannschaft | Die Teamfarbe, die den TAK-Kunden angezeigt wird. Standardmäßig ist Cyan; alle Standard-ATAK-Teamfarben sind verfügbar. |
| Rolle | Ihre TAK-Rolle. Die Auswahl besteht aus Teammitglied (Standard), Teamleiter, Hauptquartier, Scharfschütze, Sanitäter, Vorwärtsbeobachter, RTO und K9. |

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/screenshots/takIdentitySection_dark.png">
  <img src="../assets/screenshots/takIdentitySection.png" alt="TAK Identity section with Team and Role pickers">
</picture>

Eine Schaltfläche **TAK-Identität speichern** erscheint im Abschnitt nur, wenn es nicht gespeicherte Änderungen gibt. Das Speichern sendet eine Admin-Nachricht an den verbundenen Knoten; Sie sehen die Änderung in TAK-Clients im nächsten Positionsbericht.
> **Tip — Identitätswähler deaktiviert? **> Die Picker bleiben deaktiviert, bis das angeschlossene Funkgerät seine TAK-Modulkonfiguration an die App meldet. Dies geschieht normalerweise innerhalb weniger Sekunden nach der Verbindung - geben Sie ihm einen Moment oder trennen Sie ihn und verbinden Sie ihn erneut, wenn er nicht angezeigt wird.
### Serverstatus, Aktivieren und Kanal
Unterhalb des Identitätsabschnitts:
- Eine **Statusanzeige**, die anzeigt, ob der IN-App-TAK-Server läuft und ob Ihr primärer Kanal für die TAK-Nutzung geeignet ist.- Ein **TAK-Server aktivieren**-Schalter.- Ein **Channel Picker** für den LoRa-Kanal, der den Server zwischen TAK-Clients und dem Mesh verbindet.- **Nur-Lese-Modus** (behandeln Sie die App wie einen TAK-Beobachter, der CoT nicht an das Mesh weiterleitet) und **Mesh-zu-CoT-Relais** umschaltet.
> **Tip — Primäre Kanalanforderungen**> Der TAK-Server läuft im **schreibgeschützten Modus**, bis Ihr primärer Kanal einen nicht standardmäßigen Namen und einen nicht standardmäßigen 256-Bit-Verschlüsselungsschlüssel hat. Verwenden Sie die Schaltfläche **Kanal beheben** auf dem Warnbanner, um eine empfohlene TAK-Voreinstellung (Short Fast, neue AES-Taste, Name "TAK") mit einem Fingertipp anzuwenden.
### Zertifikate
Importieren Sie ein P12 (PKCS#12) oder PEM-Bundle für mTLS-geschützte ATAK/iTAK-Verbindungen. Die App speichert die verschlüsselten Zertifikate im Schlüsselbund – sie sind für andere Apps oder für die Dateifreigabe von iTunes/Finder nicht sichtbar.
### Datenpaket
Exportieren Sie einen TAK-Datenpaket-Zip, den Sie in ATAK / iTAK seitlich laden können. Der Client verwendet es, um den lokalen Server der App zu finden und ihm zu vertrauen, ohne manuell einen Host, einen Port oder ein Zertifikat einzugeben.
## Empfangsrouten
Wenn ein anderer Knoten auf dem Netz eine Route CoT sendet (`b-m-r`), Schreibt die App es automatisch als KML-Datenpaket an`Documents/TAK Routes/`Und postet eine iOS-Benachrichtigung, damit Sie sie nicht verpassen:
| Feld | Inhalt |
|-------|---------|
| Titel | Route erhalten |
| Untertitel | _Routenrufzeichen_ (oder "Unbekannte Route") |
| Körper | Gespeichert in Dateien → Meshtastic → TAK-Routen. Öffnen Sie in iTAK zum Importieren. |

iTAK ignoriert stillschweigend die Route, die CoT über seine TCP-Streaming-Verbindung erhalten hat, so dass Sie mit diesem Fallback die Route manuell importieren können. Tippen Sie auf die Benachrichtigung und navigieren Sie dann in Dateien zu **Auf meinem iPhone → Meshtastic → TAK-Routen**, teilen Sie die`.zip`Zu iTAK, und wählen Sie **Missionspaket importieren**.
> **Tip — Wo sind meine Routen? **> Die`TAK Routes`Ordner wird erstellt, wenn eine Route zum ersten Mal ankommt. Wenn Sie es nicht sehen, wurden noch keine Routen erhalten. Die KML im Inneren des Zip ist eine Standard-KML 2.2 LineString - Sie können sie auch in Google Earth oder einem beliebigen KML-Viewer öffnen.
## Wie es unter der Haube funktioniert
Sie müssen nichts konfigurieren: Die App wählt automatisch das beste TAK-Drahtformat aus, das Ihr Radio unterstützt. Firmware 2.8.0+ verwendet das neue V2-Format mit zstd-Wörterbuch-Komprimierung für reichhaltigere Nachrichtentypen und kürzere LoRa-Übertragungen. Ältere Firmware verwendet weiterhin das ältere V1-Format, das PLI und GeoChat zwischen zwei beliebigen Knoten sowie einen reichhaltigeren Apple-to-Apple-Fallback für Formen, Marker und Routen transportiert.
Entwickler und neugierige Benutzer können die vollständigen Protokolldetails in [TAK Protocol](../developer/tak-protocol.html) nachlesen.
## Fehlerbehebung
**TAK-Client verbindet sich nicht**- Stellen Sie sicher, dass der In-App-TAK-Server in **Einstellungen → TAK-Server** aktiviert ist.- Vergewissern Sie sich, dass Ihr primärer Kanal einen nicht standardmäßigen Namen **und** Verschlüsselungsschlüssel hat - ansonsten läuft der Server im schreibgeschützten Modus. Verwenden Sie **Fix Channel** im Warnbanner, falls angezeigt.- Für mTLS-Clients bestätigen Sie, dass ein P12 / PEM-Bundle unter **Zertifikate** importiert wurde.
**Routen werden in iTAK nicht angezeigt**- ITAK ignoriert absichtlich die Route CoT aus dem TCP-Streaming. Öffnen Sie die gespeicherte Zip-Datei aus **Dateien → Meshtastic → TAK-Routen** und importieren Sie sie als Missionspaket.- Wenn die`TAK Routes`Ordner fehlt, es ist noch keine Route CoT eingetroffen.
**Identitätsauswahl ist deaktiviert**- Das Funkgerät muss seine TAK-Modulkonfiguration an die App melden, bevor die Picker sie aktivieren. Verbinden Sie sich erneut, wenn es nicht innerhalb weniger Sekunden durchkommt.- Der verbundene Knoten muss die Geräterolle **TAK** oder **TAK Tracker** haben - Team / Rolle hat keine Auswirkungen auf andere Rollen.
## Anforderungen
- Firmware **2.3 oder höher** auf Ihrem Funkgerät für grundlegende TAK PLI / GeoChat; **2.8.0 oder höher** für das vollständige TAK V2-Drahtformat.- Eine ATAK / iTAK / TAK-kompatible Client-App auf Ihrem Telefon oder Tablet.- Gerät, das mit der Rolle **TAK** oder **TAK Tracker** konfiguriert ist.
