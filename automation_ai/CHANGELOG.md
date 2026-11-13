# 3.4.0

- Messengerartige Sprechblasen; Nachrichten erscheinen sofort, die Eingabe wird frei. Status und Fehler stehen im Verlauf, nächste Entwürfe bleiben erhalten.
- Zwei dauerhaft bearbeitbare Rollenprompts unter Einstellungen, einschließlich Standard-Reset. Konkrete Analyseergebnisse statt pauschaler Erledigungsmeldungen.
- Gesamte Automationsanalyse liest verfügbare native Konfigurationen und liefert Berichte pro Automation mit Befund, Quellstelle, Priorität, Vorschlag und Nutzen. Nicht geprüfte Quellen werden ausgewiesen; keine Änderungen ohne Freigabe.
- Nachrichten werden vor der Modellverarbeitung gespeichert und bei Wiederaufnahme nicht doppelt angelegt. Vorhandene Verläufe, Profile und Assist-Pipeline bleiben erhalten.
- 149 Python-Tests, 18 JavaScript-Prüfungen, Chromium und echte synthetische Qwen-Analyse erfolgreich.

# 3.3.0

- Chat beginnt direkt unter einer kompakten Kopfzeile; Modellwahl und erweiterte Parameter liegen in Einstellungen.
- Zwei dauerhafte Rollenprofile: Qwen3.5-9B / `low` für schnelle Befehle und Abfragen, Qwen3.8-27B / `xhigh` für Automationen, Skripte, Entwicklung und Diagnose.
- Automatische Zuordnung einschließlich Auftragskorrekturen; Konfigurationswerkzeuge des schnellen Modells werden vor Ausführung an das komplexe Profil übergeben. Keine stille Herabstufung bei fehlendem Reasoning-Nachweis.
- Beide Qwen-Kombinationen am realen OVH-Endpunkt mit synthetischen Daten geprüft. Kontogebundene Nachweise werden weiterhin in der Installation geprüft.
- Vorlesen: OVH als Chat-Ausgabeprofil, sichtbare Wiedergabesteuerung bei Autoplay-Sperre, längere TTS-Wartezeit und verständliche Fehler. Assist-Pipeline und Spracheingabe unverändert.
- Einmalige Profilmigration mit Sicherung der vorherigen Einstellungen; Chatverläufe und alte API-Optionen bleiben erhalten.

# 3.2.4

- OVH/Qwen-HTTP-400 beim Chat behoben: Anweisungen und lokaler Kontext stehen zusammen in genau einer ersten Systemnachricht.
- Echter Qwen3.5-9B-Test: Temperaturfrage → Sensorwerkzeug → Antwort mit synthetischem Sensor erfolgreich. Kein Modellwechsel oder reduziertes Reasoning nötig.
- Regressionstest für Chatversand, Folgefragen, Sensorantworten und Deduplizierung ergänzt.

# 3.2.3

- Fehlende EUR-Preise blockieren Chat, STT und TTS bei deaktivierten Kostenlimits nicht mehr.
- Tokens und Audiodauer werden auch ohne Preis erfasst; Kosten bleiben ausdrücklich unbekannt statt als kostenlos zu erscheinen.
- Bewusst aktivierte Kostenlimits bleiben verbindlich und melden fehlende Bewertbarkeit klar.
- Android/HTTP: kein Dateidialog mehr hinter dem Sprachbutton. „Tastatur-Diktat“ fokussiert das Textfeld und erklärt die Tastaturaufnahme. Direkte Mikrofonaufnahme benötigt weiterhin HTTPS; die native Assist-Oberfläche liefert kein Diktat an diesen Chat zurück.

# 3.2.2

- Chatversand bei lokalem HTTP repariert: keine festhängende Eingabe durch fehlendes `crypto.randomUUID`.
- OVHcloud-Klartextfehler (HTTP 422) werden bei der Reasoning-Prüfung korrekt erkannt. Anmeldung und Modellparameterfehler sind unterscheidbar.
- Laufende Chats zeigen Status und Dauer und lassen sich stoppen; Neuladen nimmt die Statusabfrage wieder auf.
- Mikrofon: direkte Aufnahme unter HTTPS; unter HTTP Audio-Dateiauswahl mit geräteabhängiger Aufnahmeoption. Native Assist-Aufnahme ist ein separater Zugang.
- Mobile M4A-/WAV-Formate unterstützt; Browser-Regressionstests jetzt Bestandteil der Release-Freigabe.

# 3.2.1

- Vorbereitete Home-Assistant-OS-Verteilung mit vorgebauten ARM64-/AMD64-Images.
- Getestete Releases veröffentlichen synchronisierte App-Metadaten und die HACS-Integration.
- Konsistente Sicherungen durch `backup: cold`; optionale Update-Automation verlangt ein Backup.
