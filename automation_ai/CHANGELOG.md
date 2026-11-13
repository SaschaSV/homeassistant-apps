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
