# Änderungen

## 0.3.0

- Kompakte Startseite ohne Werbespruch, sichtbarer KI-Chat und Navigation zu fünf Bereichen.
- Abgeschlossene Importmeldungen auf der Startseite platzsparend und schließbar; laufende
  Importe und vollständige Details behalten ihre Status- und Laufzeitanzeige.
- Eindeutige persönliche Ausgaben direkt buchen: zum Beispiel „Trage für heute 24,20 Euro
  ein für Kuchen“. Sichere Standardwerte für Datum, Profil und Haushaltsbuch; Bestätigung
  aus dem tatsächlich gespeicherten Datensatz und Rückgängig. Einfache Befehle funktionieren
  ohne KI-Anfrage. Diktierte und eingegebene Texte verwenden dieselbe Verarbeitung.
- Chat für Kategorien, Konten, Planung und echte typisierte Zusatzspalten erweitert;
  Zusatzwerte bearbeiten, exportieren und wieder importieren. Bestehende Änderungen
  bleiben revisionsgeprüft, Sammel-/Planänderungen mit Vorschau.
- Neuer Bereich Gemeinsam: Zahlende Person, frei wählbare Prozentanteile, persönliche
  Belastungen, gegenseitiger Saldo und bereits bezahlte Ausgleiche. Beide sehen nur die
  ausdrücklich gemeinsamen Vorgänge. Korrektur, Löschen, Rücknahme sowie CSV/ODS-Export.
- Optionale Hey-Nabu-Anbindung mit profilgebundenem, sperrbarem Schreibzugang und fertig
  erzeugten HA-Dateien. Sprache kann neue persönliche Buchungen erfassen; keine Finanzdaten
  abfragen. Einmalige HA-Einrichtung nötig, keine automatische Sprechererkennung.
- Vor dem Update vollständiges HA-App-Backup anlegen. Gemeinsame Daten liegen zusätzlich
  unter `/data/shared`; persönlicher ZIP-Export ersetzt dieses Vollbackup nicht. Private
  0.2.x-Daten werden nicht erneut aufgeteilt oder automatisch gemeinsam sichtbar.

## 0.2.1

- Importstatus direkt sichtbar: Laufzeit des Imports und einzelner KI-Aufrufe, Arbeitsschritt,
  Anfragezeitlimit, Zeilenzahlen und eindeutiger Abschluss, auch bei null neuen Einträgen.
- Laufende Importe bleiben nach Schließen des Fensters oder Neuladen auffindbar;
  unterbrochene Statusverbindungen werden erklärt und erneut abgefragt.
- Abbrechen beendet den laufenden Aufruf; Fortsetzen bleibt möglich. Neue Uploads warten
  nicht hinter einer langen KI-Anfrage. Gesamtzeitlimit je Modellanfrage wird durchgesetzt.
- CSV-/ODS-Exporte aller Datenbereiche sowie ZIP-Sicherung/-Wiederimport melden Fortschritt
  und Abschluss. Eigene Tabellenexporte werden ohne unnötigen KI-Aufruf erkannt.

## 0.2.0

- Jahresbudget mit monatlichen/jährlichen Einnahmen und Ausgaben, Monatsbedarf und Sparüberschuss.
- ODS-Budgetimport mit Abschnitten ohne Datumsangaben; formatierte Leerbereiche verträglich lesen.
- Importfortschritt und vorübergehende KI-Störungen sichtbar anzeigen; Fortsetzen möglich.
- Private Datenbestände nach HA-Anmeldung; zweites Profil heißt Saskia.
- Einmalige anteilige Übernahme bestehender Einträge, vollständiges geschütztes Altarchiv.
- Vor Update vollständiges HA-App-Backup erstellen. Bankzugänge je Person neu verbinden;
  Saskia richtet ihre eigene KI-Verbindung ein. Rollback nur mit passendem Vollbackup.

## 0.1.1

Erste für die Installation vorgesehene Version. Der private Multiarch-Index bewahrt
Repository-, Versions- und Commitangaben auch für native Docker-Images. Die geprüften
Images werden über unveränderte Digests referenziert; vorhandene Versionstags werden
niemals überschrieben. Die Katalogfreigabe verlangt private Sichtbarkeit, authentifizierten
Pull und verweigerten anonymen Zugriff.

## 0.1.0

Erste Implementierung nach Spezifikation 2.1: Profile/Haushalt, Planung, Haushaltsbuch,
Vermögen, intelligente CSV/ODS/XLSX-Imports, CSV/ODS-/Datenexport mit Restore,
Qwen3.8-27B/xhigh, direktes OVHcloud-Diktat und lesender Sparkassen-FinTS-Connector.
Private Multiarch-Verteilung und geführte Terminal-Einrichtung. Bestehender Registry-
Zugang und andere Apps bleiben erhalten; Besitzer-ID aus der lokalen HA-Konfiguration
erkennen oder gezielt eingeben. Eigene Bank-/OVH-Zugänge sind optional einzurichten.
