# Finanzhaushalt AI

Deutschsprachiger Finanzhaushalt für Home Assistant OS: Planung, Haushaltsbuch,
Kontostände, CSV/ODS/XLSX-Import, Diktat und optionale OVHcloud-KI.

**Einrichtung mit einem eingefügten Befehlsblock:** Die [geführte Terminal-Anleitung](DOCS.md#geführte-installation-im-vorhandenen-terminal)
übernimmt Katalog, Installation und Besitzerzuordnung. Ein vorhandener Registryzugang
bleibt erhalten. Fehlt er, wird ein GitHub-Lesetoken verdeckt abgefragt.

Das Container-Package ist privat. Vor der Installation `ghcr.io` im Supervisor
mit einem berechtigten PAT classic (`read:packages`) konfigurieren. Bestehende
Zugriffe anderer Apps dabei erhalten. Quellcode wird nicht im App-Katalog verteilt.

Nach Installation die HA-Nutzer-ID der Besitzerperson in `owner_user_id` setzen.
Ohne gültige Ingress-Identität besteht kein Zugriff. Eine zweite HA-Person kann
anschließend ausdrücklich freigeschaltet werden. Beide sehen den gemeinsamen Bestand.

Standardmodell: **Qwen3.8-27B**, `reasoning_effort: xhigh`. Manuelle Funktionen
arbeiten unabhängig von KI und Bankzugang. Details: [Benutzeranleitung](DOCS.md).

Architekturen: aarch64 (Pi 4 mit 64-Bit-HAOS), amd64. Diese Metadaten werden erst
nach erfolgreichen Architektur-Builds und geprüftem privaten Imagezugriff veröffentlicht.
