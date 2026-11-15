# Finanzhaushalt AI

Deutschsprachiger Finanzhaushalt für Home Assistant OS: Planung, Haushaltsbuch,
Kontostände, CSV/ODS/XLSX-Import, Diktat, gemeinsames Ausgabenbuch und optionale OVHcloud-KI.

**Installation über den Home-Assistant-App-Store**, wie bei Automation AI:
[App-Store öffnen](https://my.home-assistant.io/redirect/supervisor_store/) → Katalog
aktualisieren → **Finanzhaushalt AI → Installieren**. Der einmalige private Registryzugang
lässt sich ebenfalls über **⋮ → Registries** einrichten. Kein Terminal erforderlich.
[Schritt-für-Schritt-Anleitung](DOCS.md#installation-über-den-home-assistant-app-store).

Das Container-Package ist privat. Vor der Installation `ghcr.io` im Supervisor
mit einem berechtigten PAT classic (`read:packages`) konfigurieren. Bestehende
Zugriffe anderer Apps dabei erhalten. Quellcode wird nicht im App-Katalog verteilt.

Nach Installation die HA-Nutzer-ID der Besitzerperson in `owner_user_id` setzen.
Ohne gültige Ingress-Identität besteht kein Zugriff. Eine zweite HA-Person kann
anschließend ausdrücklich freigeschaltet werden. Jede HA-Anmeldung sieht ihr eigenes
Finanzprofil und die ausdrücklich unter **Gemeinsam** eingetragenen Vorgänge. Andere
private Buchungen bleiben unsichtbar. Kostenanteile und gegenseitiger Ausgleich werden
centgenau berechnet.

Auf der kompakten Startseite öffnet **KI-Chat** die Eingabe zum Schreiben oder Diktieren.
Eindeutige Ausgaben wie „Trage für heute 24,20 Euro ein für Kuchen“ werden direkt gespeichert.
Kategorien und eigene Zusatzspalten lassen sich ebenfalls per Chat anlegen. Unter
**Einstellungen → Hey Nabu** erzeugt die App die optionale HA-Einrichtung für Sprachbuchungen.

Standardmodell: **Qwen3.8-27B**, `reasoning_effort: xhigh`. Manuelle Funktionen
arbeiten unabhängig von KI und Bankzugang. Details: [Benutzeranleitung](DOCS.md).

Architekturen: aarch64 (Pi 4 mit 64-Bit-HAOS), amd64. Diese Metadaten werden erst
nach erfolgreichen Architektur-Builds und geprüftem privaten Imagezugriff veröffentlicht.
