# Änderungen

## 1.0.4

- Speichern und Öffnen der Formulare funktionieren auch ohne `crypto.randomUUID`, etwa in älteren Android-WebViews und bei lokalem HTTP-Zugriff.
- Modellprofile zeigen beim erneuten Öffnen die gespeicherten Körperdaten; eine wiederholte Anfrage nach Verbindungsabbruch erzeugt keine doppelte Profilversion.
- Vorlagen zeigen Menge und Stärke sowie beschriftete Aktionen zum Bearbeiten, Ein-/Ausblenden und Sortieren. Die Reihenfolge wird gemeinsam und konfliktsicher gespeichert.
- Dateiauswahl ohne einschränkenden Android-Typfilter; klare Trennung zwischen Hinzufügen und vollständiger JSON-Wiederherstellung mit Vorschau.
- Alcotrack-Pro-Tagesexport mit Tage, Getränke und Alkoholkonsum [g] wird als Tagesmenge importiert. Ausgaben werden ignoriert. Keine erfundenen ml, Vol.-% oder Uhrzeiten; wiederholter Import erkennt bereits übernommene Tage.
- Tagesmengen bleiben in Auswertungen, Bearbeitung, Rücknahme und Export erhalten. Nullmengen bestätigen nicht automatisch alkoholfreie Tage.
- Datenbank- und JSON-Format unterstützen Tagesmengen; bestehende Daten werden erhalten und vor dem Update gesichert. Alte JSON-Exporte bleiben lesbar.

## 1.0.3

## Erste vollständige Anwendung

- Deutsches Tagebuch mit bearbeitbaren Getränkevorlagen, Komponentenmodus, Nachträgen und Rücknahme.
- Jahresübersicht, Kalender, freie Zeiträume, Abdeckung und nachvollziehbare Durchschnittswerte.
- Optionale Modellkurve mit Aufnahme, gemeinsamem Abbau und versionierten Profilen.
- OVHcloud-Texteingabe mit validierten Aktionen, atomaren Batches und Idempotenz.
- CSV/JSON-Transfer, Importvorschau, Sicherungspunkte und geschützter Ingress-Zugang.
- Automatische geprüfte Bereitstellung privater ARM64-/AMD64-Images.
- Vergrößerte Schrift, verschachtelte Ingress-Pfade und Netzwerkwiederholungen ohne Doppelbuchung geprüft.
- Tagesdetails und Revisionsprüfung nach Rücknahmen verbessert; Changelogversion und Historie werden automatisch ergänzt.
- Sicherungspunkte werden nach tatsächlichem Alter aufbewahrt; alte zeitlich irrelevante Einträge blockieren keine neue Modellkurve.
- Bei Mischgetränken zeigt die KI-Rückmeldung die tatsächlichen Bestandteile und bekannte Mengen.
- Registry-Verbindungs- und Authentifizierungsfehler stoppen die Veröffentlichung, statt vorhandene Tags als fehlend zu behandeln.

Keine Installation oder Aktualisierung auf Home Assistant ohne Auswahl des Nutzers.
