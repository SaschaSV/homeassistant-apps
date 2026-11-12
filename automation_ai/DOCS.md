# Erster Start

1. Den vollständigen Ordner `local_apps/automation_ai` in die lokale App-Freigabe kopieren, Store aktualisieren, installieren und starten.
2. Den mitgelieferten Integrationsordner nach `config/custom_components/automation_ai` kopieren und Core neu starten.
3. App über Ingress als persönlicher HA-Administrator öffnen. In Einstellungen einen Kopplungsschlüssel erzeugen; die Integration Automation AI mit Host `local-automation-ai` und diesem Schlüssel hinzufügen.
4. OVH-Schlüssel geschützt hinterlegen, Modellkatalog laden, Schnell-/Planungsmodelle auswählen und Preise/Monatslimits eintragen. Unbekannte Sprachpreise sperren die jeweilige Cloudfunktion.
5. Den eigenen Android-Mikrofonweg über einen erlaubten HTTPS-/Ingress-Zugang prüfen. Okay Nabu ist ein separater Weg.
6. Verwaltungsfunktionen erst für die Nutzung aktivieren. Jeder Verwaltungsplan benötigt weiterhin eine konkrete Freigabe. Optionale reale Tests sind nicht vorgewählt.

Die App hat technisch breite Core-/Supervisor-Rechte. Konkrete Werkzeuge begrenzen deren Verwendung; ein Modelltext ist keine Freigabe. Kritische Geräteaktionen sind standardmäßig gesperrt. Es gibt keinen öffentlichen Host-Port, keinen freien Shell-/Datei-/HTTP-Agenten und keinen stillen Cloudanbieterwechsel.

Die vollständige Android-Anleitung, Bedienung, Grenzen und tatsächlichen Testnachweise liegen im gelieferten ZIP. Bei unklarem Ausgang zuerst den gespeicherten Auftrag abgleichen; nicht wiederholt denselben Gerätebefehl senden. Ein gemeldeter HA-Zustand allein beweist keine physische Wirkung.

## Installation und Updates über den App-Store

Repository: https://github.com/SaschaSV/homeassistant-apps

Automation AI installieren und starten. Für die Bridge die Integration separat über HACS aus https://github.com/SaschaSV/automation-ai-integration installieren und Core neu starten. Beide Artefakte haben dieselbe Versionsnummer; eine neue Installation benötigt zuerst ein veröffentlichtes Release mit öffentlich abrufbarem Image.

Vor einem App-Update „Backup“ auswählen. Für verpflichtende Backups kann die mitgelieferte Blueprint-Vorlage im Entwicklungsrepository verwendet werden; native automatische Updates dann deaktivieren, damit sie den Backup-Pfad nicht umgehen. `backup: cold` stoppt die App während der Sicherung und sichert SQLite samt Tresorschlüssel konsistent. Diese Einstellung allein erzwingt kein Backup bei jedem Update.
