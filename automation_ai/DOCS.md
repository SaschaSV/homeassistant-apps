# Erster Start

1. Den vollständigen Ordner `local_apps/automation_ai` in die lokale App-Freigabe kopieren, Store aktualisieren, installieren und starten.
2. Den mitgelieferten Integrationsordner nach `config/custom_components/automation_ai` kopieren und Core neu starten.
3. App über Ingress als persönlicher HA-Administrator öffnen. In Einstellungen einen Kopplungsschlüssel erzeugen; die Integration Automation AI mit Host `local-automation-ai` und diesem Schlüssel hinzufügen.
4. OVH-Schlüssel geschützt hinterlegen, Modellkatalog laden, Schnell-/Planungsmodelle auswählen und Preise können optional zur EUR-Auswertung eingetragen werden; Monatslimits sind ebenfalls optional. Unbekannte Preise werden als unbekannte Kosten erfasst und blockieren ohne aktiviertes Limit keine Cloudfunktion.
5. „Sprechen“ startet unter HTTPS mit Mikrofonfreigabe die direkte Aufnahme. Bei lokalem HTTP fokussiert „Tastatur-Diktat“ das Eingabefeld. Dort das Mikrofon der Android-Tastatur antippen und anschließend den erkannten Text senden. Native Assist-/Okay-Nabu-Aufnahme ist ein separater Weg.
6. Verwaltungsfunktionen erst für die Nutzung aktivieren. Jeder Verwaltungsplan benötigt weiterhin eine konkrete Freigabe. Optionale reale Tests sind nicht vorgewählt.

Die App hat technisch breite Core-/Supervisor-Rechte. Konkrete Werkzeuge begrenzen deren Verwendung; ein Modelltext ist keine Freigabe. Kritische Geräteaktionen sind standardmäßig gesperrt. Es gibt keinen öffentlichen Host-Port, keinen freien Shell-/Datei-/HTTP-Agenten und keinen stillen Cloudanbieterwechsel.

Die vollständige Android-Anleitung, Bedienung, Grenzen und tatsächlichen Testnachweise liegen im gelieferten ZIP. Bei unklarem Ausgang zuerst den gespeicherten Auftrag abgleichen; nicht wiederholt denselben Gerätebefehl senden. Ein gemeldeter HA-Zustand allein beweist keine physische Wirkung.

## Installation und Updates über den App-Store

Repository: https://github.com/SaschaSV/homeassistant-apps

Automation AI installieren und starten. Für die Bridge die Integration separat über HACS aus https://github.com/SaschaSV/automation-ai-integration installieren und Core neu starten. Beide Artefakte haben dieselbe Versionsnummer; eine neue Installation benötigt zuerst ein veröffentlichtes Release mit öffentlich abrufbarem Image.

Vor einem App-Update „Backup“ auswählen. Für verpflichtende Backups kann die mitgelieferte Blueprint-Vorlage im Entwicklungsrepository verwendet werden; native automatische Updates dann deaktivieren, damit sie den Backup-Pfad nicht umgehen. `backup: cold` stoppt die App während der Sicherung und sichert SQLite samt Tresorschlüssel konsistent. Diese Einstellung allein erzwingt kein Backup bei jedem Update.

## Wenn der Chat oder die Modellprüfung nicht antwortet

Ab 3.2.2 funktioniert der Chatversand auch bei lokalem HTTP. Laufende Anfragen zeigen Dauer und Verarbeitungsphase. „Stoppen“ beendet die Chatverarbeitung; bereits ausgeführte Geräteaktionen werden nicht zurückgenommen. Ein erneutes Öffnen der App lädt den Status, ohne denselben Befehl erneut zu senden.

Nach dem Update einmal unter Einstellungen „Modellkatalog aktualisieren“ wählen, falls die frühere Reasoning-Prüfung abgebrochen wurde. HTTP 422 bei einer absichtlich ungültigen Teststufe wird intern ausgewertet und ist kein Nachweis eines falschen Schlüssels. Erst erfolgreiche Modellprüfungen geben Reasoning-Stufen frei; xhigh wird nicht still auf eine niedrigere Stufe gesetzt. Bei HTTP 401/403 Schlüssel und Modellberechtigung prüfen. Der gespeicherte Schlüssel muss für dieses Update nicht erneut eingegeben werden.

Der OVHcloud-STT-Preis samt Quelle und Datum ist bei deaktivierten Limits optional. Bis zu 85 Sekunden / 5 MB aufnehmen. Eine native Aufnahme im Dateidialog kann nicht für jede Companion-App-/Betriebssystemversion zugesichert werden. HTTPS-Direktaufnahme setzt weiterhin die Mikrofonfreigabe voraus.

## Preise und Diktieren ab 3.2.3

Bei ausgeschalteten Kostenlimits ist keine Preiskonfiguration erforderlich. Ein bewusst aktiviertes Kostenlimit verlangt dagegen bewertbare EUR-Kosten; deaktivieren unter Automation AI → Zahnrad → Reasoning, Tokenlimits und optionale Kostenbegrenzung (Limitfeld leeren). Fehlende Preise und frühere unbewertete Anfragen werden nicht als kostenlos behandelt.

Androids native Assist-Aufnahme kann nicht direkt als Diktierfunktion für diese eingebettete Webseite benutzt werden. Unter HTTP wird keine Dateiauswahl mehr als Aufnahme gestartet. Für die direkte Aufnahme muss der in der Companion-App verwendete Home-Assistant-Zugang HTTPS mit gültigem Zertifikat verwenden (beispielsweise ein bereits eingerichteter Home Assistant Cloud-Zugang). Danach dieselbe Automation-AI-App öffnen und die Mikrofonberechtigung erteilen. Die App richtet HTTPS nicht selbst ein und ändert keine Assist-Pipeline. Ohne HTTPS bleibt das Mikrofon der Android-Tastatur nutzbar; dessen Spracherkennung gehört zum konfigurierten Tastaturdienst.

## Qwen-HTTP-400 beim Chat (behoben in 3.2.4)

Qwen3.5-9B lehnte die bisherige zweite Systemnachricht mit „System message must be at the beginning“ ab. Version 3.2.4 übermittelt Anweisungen und lokalen Kontext gemeinsam in einer ersten Systemnachricht. Nach dem Update die Oberfläche neu öffnen und die Frage erneut senden. Weder Schlüssel noch Modellkatalog noch Tokenlimit müssen dafür neu eingerichtet werden. Frühere fehlgeschlagene Anfragen werden nicht automatisch erneut gesendet.
