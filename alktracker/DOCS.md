# Alktracker einrichten und benutzen

## Installation

1. Home Assistant OS mit 64-Bit-System prüfen: Einstellungen → System → Reparaturen →
   Systeminformationen. `aarch64` oder `amd64` ist erforderlich.
2. Einen separaten GitHub PAT **classic** mit `read:packages` und Leserecht auf das
   private Package `saschasv/alktracker` erstellen. Er braucht keine Schreibrechte.
   In der Supervisor-Registryverwaltung `ghcr.io` mit GitHub-Benutzername und diesem
   Token hinterlegen. Vorhandene Zugänge für andere Apps erhalten. Je nach HA-Version
   ist die Registryverwaltung über die Supervisor-CLI `ha docker registries` zugänglich;
   die dortige Hilfe prüfen. Token niemals in Chat, Screenshots oder öffentliche Dateien geben.
3. Bestehenden App-Katalog `https://github.com/SaschaSV/homeassistant-apps` hinzufügen
   oder aktualisieren. Alktracker installieren.
4. In der geschützten App-Konfiguration `owner_id` auf die stabile ID deines
   Home-Assistant-Benutzers setzen (Benutzerverwaltung im erweiterten Modus;
   Benutzer öffnen und dessen ID verwenden). Nicht Benutzername oder Anzeigename.
   Die App bleibt ohne passende ID gesperrt. Es gibt keinen automatischen Erstbesitzer.
5. Starten und über **Weboberfläche öffnen** aufrufen. Keine Portfreigabe nötig.
6. Alktracker in den vorhandenen Home-Assistant-Backupplan aufnehmen. Die App benutzt
   kalte Backups; das richtet noch keinen Zeitplan ein.

Bei `403`: Ingress verwenden, Besitzer-ID kontrollieren, HA/Supervisor aktualisieren
und Ingress-Sitzung neu öffnen. Fehlende Identitätsheader erlauben ausdrücklich keinen Zugriff.

## Erfassen und auswerten

Eine Vorlage öffnet ein Bearbeitungsblatt. Menge, Stärke, Anzahl und Datum prüfen und
speichern. Mehrere gleiche Getränke werden einzeln gespeichert und können einzeln
korrigiert werden. Eine Eintragsänderung verändert keine Vorlage. Unter Einstellungen → Vorlagen öffnet **Bearbeiten** die Standardwerte. **Nach oben / Nach unten** ändert die Reihenfolge auch in der Schnellauswahl; **Ausblenden** entfernt eine Vorlage aus der Schnellauswahl, ohne Einträge zu löschen. **Als Standard
speichern** verändert nur zukünftige Vorbelegungen. Mischgetränke werden entweder aus
Bestandteilen oder anhand Gesamtmenge und Gesamtstärke berechnet, nie doppelt.

Nur ein Datum bekannt: Uhrzeit unbekannt lassen. Für Intervalle werden Mengen nach
tatsächlicher Dauer auf lokale Tage verteilt. Mehrdeutige Uhrzeiten während der
Zeitumstellung erfordern die Auswahl der ersten oder zweiten Uhrzeit.

**Alles erfasst** schließt einen Tag/Zeitraum ab. Leere Tage darin werden ausdrücklich
als alkoholfrei bestätigt. Ein leeres Tagebuch allein und ein 0,0-%-Getränk beweisen
keinen alkoholfreien Tag. Neue Einträge öffnen den Tagesabschluss wieder.

Auswertungen nennen ihre Abdeckung und Nenner. Durchschnitte vollständiger Tage
berücksichtigen ausschließlich deren Mengen. Monatsdurchschnitte benötigen ganze,
abgeschlossene und vollständig erfasste Monate. Änderungen und Rücknahmen stehen
unter Einstellungen → Daten & Sicherung. Bei späteren Änderungen wird eine veraltete
Rücknahme abgewiesen, statt Daten zu überschreiben.

## Optionale KI

In Einstellungen → KI die Übermittlung bestätigen, den OVH-Key eingeben und speichern.
Anschließend **Verbindung testen**. Vorgabe: `Qwen3.8-27B`, `xhigh`, offizieller
OpenAI-kompatibler OVH-Endpunkt. Tests prüfen Tool Calling und strukturierte Ausgabe.
Ein Fehler führt nicht zum stillen Modellwechsel. Das angegebene Tokenbudget begrenzt
Anfragen; fehlende Tarifdaten werden als unbekannte Kosten ausgewiesen.

Beispiel: „Heute zwei Bier, jeweils 330 ml, 5 Prozent.“ Fehlende Uhrzeit verhindert
die Gramm-Erfassung nicht. „Gestern zwei Weißwein, nimm die Standardangaben“ benutzt
die gespeicherten Vorlagen. Klare Buchungen werden direkt gespeichert und bieten
Rückgängig. Größere Löschungen brauchen eine konkrete gemeinsame Bestätigung.

Übertragen werden Anfrage, Datum, nötige Vorlagen und gegebenenfalls gezielt gesuchte
Einträge. Körperdaten und die gesamte Historie sind kein Standardkontext. Direkte
Audioaufnahme benötigt HTTPS, Browserfreigabe und einen passenden OVH-STT-Endpunkt.
Unterstützung auf dem konkreten Android-WebView muss getestet werden. Text und
Android-Tastaturdiktat bleiben nutzbar. Es wird keine Browser-/Google-Spracherkennung aufgerufen.

## Optionale Alkoholkurve

Erst bei Nutzung ein Modellprofil anlegen. **Profil speichern** bestätigt die Speicherung; beim nächsten Öffnen sind Gewicht, Größe und Alter wieder vorausgefüllt. Watson benötigt Gewicht, Größe, Alter und
Standdatum; der manuelle Widmark-Modus nur Gewicht und Verteilungsfaktor. Die historische
Watson-Einteilung beschreibt körperbezogene Gruppenformeln, keine Geschlechtsidentität.
Kein Modus ist persönlich kalibriert. Default: Abbau 0,15 ‰/h, Aufnahmefenster 60 Minuten,
verfügbarer Anteil 1,0. Diese Werte sind Szenarioannahmen.

Den vollständigen Trinkverlauf und einen nüchternen Modellstart ausdrücklich bestätigen.
Datumseinträge ohne Zeit im relevanten Zeitraum verhindern einen aktuellen Gesamtwert.
Die Bestätigung muss aktuell sein. Profilrevisionen bleiben erhalten. Änderungen gelten
für die Zukunft; historische Parameter werden nicht überschrieben. Die Grammstatistik
hängt niemals vom Modellprofil ab.

Die normierte Aufnahme wird über alle Getränke addiert und eine einzige Abbaurate auf
den gemeinsamen nichtnegativen Zustand angewendet. Es gibt keinen Reset um Mitternacht
und keine vorab angesammelte Abbau-Gutschrift. Prognosen nehmen keine weiteren Getränke an.
**Modellschätzung – keine Messung und keine Aussage zur Fahrtüchtigkeit.**

## Export, Import und Wiederherstellung

Auswertung → Export verwendet den gewählten Zeitraum inklusive Start und Ende.
Einstellungen → Daten & Sicherung bietet den Gesamtexport. JSON enthält Ereignisse,
Vorlagen, Tagesabdeckung und Profilhistorie. CSV enthält Ereignisse und Tagesabdeckung.
Normale Exporte enthalten keine Keys. Dateien mit Trink-/Profildaten privat aufbewahren.

**Einträge hinzufügen** übernimmt CSV- oder Alktracker-JSON-Daten zusätzlich zum bestehenden
Bestand. **Alktracker-Sicherung wiederherstellen** ersetzt dagegen alle Einträge,
Tagesabschlüsse, Vorlagen und Profile durch einen vollständigen Alktracker-JSON-Export;
KI-Einstellungen und Schlüssel bleiben erhalten. Eine CSV ist keine vollständige Sicherung.

Datei auswählen, **Vorschau anzeigen** drücken, Angaben prüfen und erst dann bestätigen.
Beim Wiederherstellen zusätzlich das Ersetzen des Bestands bestätigen. Der Dateidialog
zeigt alle Dateitypen, damit Android CSV-Dateien nicht wegen eines falschen Dateityps
ausgraut; die App prüft den tatsächlichen Inhalt.

**Alcotrack Pro:** Der Tagesexport mit den Überschriften `Tage`, `Getränke`,
`Alkoholkonsum [g]` und optional `Ausgaben [€]` wird direkt erkannt. Ausgaben werden
ignoriert. Tabulatoren, Semikolon oder Komma als Trennzeichen und Dezimalkomma sind erlaubt.
Datumswerte dürfen YYYY-MM-DD, DD.MM.YYYY oder DD/MM/YYYY verwenden. Die Grammmenge
bleibt eine Tageszusammenfassung mit Getränkeanzahl; einzelne ml, Vol.-% und Uhrzeiten
werden nicht erfunden. Die Mengen erscheinen in den Auswertungen und lassen sich im
Tagebuch korrigieren. 0 g bestätigt keinen alkoholfreien Tag; den Tagesabschluss bei
Bedarf selbst bestätigen. Bereits vorhandene einzelne Einträge am selben Tag würden
zusätzlich zählen – daher vor Bestätigung auf Überschneidungen achten.
 Identische IDs werden
dedupliziert. Abweichende vorhandene IDs werden nicht still überschrieben. Für eine
vollständige Wiederherstellung den vollständigen JSON-Export wählen; ein Zeitraumexport
ist dafür ungeeignet. Vor Import wird automatisch ein konsistenter lokaler
SQLite-Sicherungspunkt erstellt. Bis zu acht Punkte bleiben in `/data/backups` erhalten.

Ein Home-Assistant-App-Backup sichert `/data` einschließlich Einstellungen und Secrets.
Zum vollständigen Rückweg ein passendes HA-Backup mit der zugehörigen App-Version
wiederherstellen. Für einen manuellen Datenbank-Rückweg App stoppen, `/data` sichern,
passendes SQLite-Backup anstelle der Datenbank einsetzen und erst dann mit passender
Imageversion starten. Veraltete WAL-/SHM-Dateien gehören nicht zu einer Backup-API-Datei.
Die App verweigert inkompatible Datenbank-Downgrades und startet keine leere Ersatzdatenbank.

## Updates

Updates ausschließlich über Home Assistant installieren. Vor größeren Änderungen ein
HA-Backup erstellen. Die neue Version erscheint nach Aktualisierung des App-Katalogs;
das normale Prüfintervall kann die Anzeige verzögern. Alktracker aktualisiert keine
Pakete oder eigenen Programmcode zur Laufzeit. Alte private Images nicht löschen,
solange die dazugehörigen Backups noch benötigt werden.
