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

**Tage ohne eingetragene Alkoholmenge gelten bis einschließlich heute automatisch als
alkoholfrei.** Es gibt keine manuelle Bestätigung und keinen Tagesabschluss mehr.
Ein nachträglich gespeichertes Getränk ändert Tagesstatus, Kalender, Statistiken und
Streak sofort. Beim Löschen der letzten Alkoholmenge wird der Tag wieder alkoholfrei.
Zukünftige Tage zählen nicht mit.

Das Dashboard zeigt die aktuelle alkoholfreie Folge bis heute, auch über den
Jahreswechsel hinweg. Der heutige Tag zählt mit, solange kein Alkohol eingetragen ist;
ein Eintrag heute setzt den aktuellen Streak auf 0. Die längste Folge bezieht sich
auf den gewählten Auswertungszeitraum. Ohne bisherigen Verlauf beginnt die aktuelle
Folge am 1. Januar des laufenden Jahres.

Ein **Bieräquivalent entspricht 20 g Reinalkohol**, ungefähr 500 ml Bier mit 5 Vol.-%.
Die Anzeige wird wie Gramm- und Durchschnittswerte auf ganze Zahlen gerundet.
Exakte Eingaben, Berechnungen und Exporte bleiben unverändert präzise. Die geschätzte
Promillekurve und ihre Modellparameter behalten die erforderlichen Nachkommastellen.

Das Wochentagsmuster zeigt je Wochentag Gesamtmenge, durchschnittliche Gramm je Tag und
Trinktage im Verhältnis zu allen Tagen im Zeitraum bis heute. Tage mit 0 g fließen in
den Durchschnitt ein. Monatsdurchschnitte verwenden abgeschlossene Kalendermonate.

## Optionale KI

In Einstellungen → KI die Übermittlung bestätigen, den OVH-Key eingeben und speichern.
Anschließend **Verbindung testen**. Vorgabe: `Qwen3.8-27B`, `xhigh`, offizieller
OpenAI-kompatibler OVH-Endpunkt. Tests prüfen Tool Calling und strukturierte Ausgabe.
Ein Fehler führt nicht zum stillen Modellwechsel. Der Timeout in Sekunden hat keine
obere Eingabegrenze. Während einer KI-Anfrage zeigt die App die verstrichenen Sekunden;
der Auftrag läuft im Hintergrund und sein Status wird regelmäßig abgefragt. Abbrechen
bleibt möglich. Die Server und Netze des Anbieters können eigene Grenzen haben. Das angegebene Tokenbudget begrenzt
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
Einstellungen → Daten & Sicherung bietet den Gesamtexport. Nach dem Erstellen öffnet
sich **Export speichern** mit **Datei herunterladen** sowie, wenn vom Browser
unterstützt, **Datei teilen / speichern**. Falls die Home-Assistant-Webansicht keinen
Download öffnet, zeigt **Exporttext anzeigen** den vollständigen Text zum Auswählen
und Kopieren. Die App bleibt dabei geöffnet und zeigt Exportfehler verständlich an. JSON enthält Ereignisse,
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
Tagebuch korrigieren. Tage mit 0 g gelten automatisch als alkoholfrei. Bereits vorhandene einzelne Einträge am selben Tag würden
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

## Analysebericht und gesundheitliche Einordnung

Unter **Auswertung → Bericht & Gesundheit** oder über die entsprechende Karte auf der
Übersicht öffnet sich der Bericht. **Gesamter Verlauf** betrachtet die Zeit vom ersten
vorhandenen Eintrag bis heute. **Gewählten Zeitraum auswerten** verwendet die Von-/Bis-
Auswahl. Alle Werte werden aus dem aktuellen Bestand neu berechnet; frühere PDF-Werte
werden nicht als zusätzliche Buchungen übernommen.

Der Bericht enthält Jahres- und Monatsmengen, Trinktage und deren Verteilung, dieselben
Vergleichsfenster pro Jahr, hohe Tagesmengen, Jahreskalender und die sechs längsten
Trinkpausen. Teiljahre, Teilmonate und ein zusätzlicher Schalttag bleiben sichtbar.
Pausen zählen beide Datumsgrenzen; Folgen an einer Auswahlgrenze sind entsprechend
markiert. Wie im Tagebuch gelten fehlende Alkoholmengen bis heute als alkoholfrei.
Nicht eingetragene Getränke würden deshalb Mengen unterschätzen und Pausen überschätzen.

Das **aktuelle Risikoprofil** endet immer heute, unabhängig vom gewählten historischen
Berichtszeitraum. Es zeigt die letzten 28 und 90 Tage. Bei kürzerem Verlauf wird das
Fenster gekürzt; vor sieben Verlaufstagen erfolgt keine DGE-Einstufung. Wochenmittel =
Gesamtgramm geteilt durch Kalendertage mal sieben. Das ist keine tatsächlich typische
Woche. Hohe Tagesmengen und die höchste Sieben-Tage-Summe werden deshalb separat gezeigt.

Die DGE-Mengenbereiche dienen als Orientierung für gesunde, nicht schwangere oder
stillende Erwachsene: unter 27 g/Woche risikoarmer Bereich, 27–81 g moderater Bereich,
über 81 g hoher Bereich. Kein Bereich ist gesundheitlich risikofrei. Gerundete Anzeigen
ändern die Einordnung nicht; sie benutzt exakte Werte. Die WHO-Definition ab 60 g
bezieht sich auf eine Trinkgelegenheit. Tagesmengen ab 60 g sind hier ein Mengenhinweis
und keine nachgewiesenen Rauschtrink-Episoden. Die zusätzliche 80-g-Marke beschreibt
die Daten und ist keine weitere medizinische Grenze.

Die Einordnung nutzt ausschließlich das Konsummuster. Körperprofile, Krankheiten und
Medikamente fließen nicht ein. Es werden keine Organschäden diagnostiziert und keine
individuellen Erkrankungswahrscheinlichkeiten oder Abhängigkeitsdiagnosen berechnet.
Quellen, Geltungsbereich und Fachstand stehen am Berichtende. Die App recherchiert nicht
selbständig im Hintergrund; die hinterlegten Empfehlungen werden mit App-Updates geprüft.

**Drucken / als PDF** öffnet den Druckdialog des Browsers, soweit die Webansicht dies
unterstützt. **Bericht als Datei** erstellt einen HTML-Bericht mit Kalendern und Tabellen,
der ohne Internetverbindung lesbar und in einem Browser als PDF druckbar ist. Persönliche
Berichte wie Tagebuchexporte privat aufbewahren. Für die Berichtserstellung wird keine
KI-/Cloudverbindung benötigt.

Fachquellen (geprüft 16.11.2026):

- [DGE: FAQ Alkohol](https://www.dge.de/gesunde-ernaehrung/faq/alkohol/)
- [DGE: Alkoholkonsum in Deutschland, 2026](https://www.dge.de/presse/meldungen/2026/alkoholkonsum-in-deutschland/)
- [WHO: Alcohol](https://www.who.int/news-room/fact-sheets/detail/alcohol)
- [WHO: Heavy episodic drinking](https://www.who.int/data/gho/indicator-metadata-registry/imr-details/459?lang=en)
