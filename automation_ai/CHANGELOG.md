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
