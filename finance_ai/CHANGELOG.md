# Änderungen

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
