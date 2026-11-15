# Einrichten und bedienen

## Installation über den Home-Assistant-App-Store

Die Installation erfolgt über die Oberfläche, wie bei Automation AI. Ein Terminal
ist für diesen Weg nicht erforderlich. Voraussetzung: Home Assistant ab 2026.11.0
auf 64-Bit-HAOS (aarch64 oder amd64).

1. **Einstellungen → Apps → App installieren / App-Store (Marketplace)** öffnen.
   [App-Store direkt öffnen](https://my.home-assistant.io/redirect/supervisor_store/).
2. Über **⋮ → Nach Updates suchen / Aktualisieren** den Katalog neu laden.
   Das vorhandene Repository `https://github.com/SaschaSV/homeassistant-apps`
   enthält jetzt auch **Finanzhaushalt AI**. Falls es fehlt: **⋮ → Repositories**
   öffnen und diese URL hinzufügen.
3. Den privaten Imagezugang einmalig über **⋮ → Registries / Registrierungen**
   einrichten: **Registry hinzufügen**, Server `ghcr.io`, Benutzer `SaschaSV`,
   Passwort ein [GitHub PAT classic mit read:packages](https://github.com/settings/tokens/new?description=Home%20Assistant%20Finance%20AI&scopes=read:packages).
   Den Token ausschließlich in dieses Passwortfeld eingeben. Einen bereits passenden
   `ghcr.io`-Eintrag verwenden; bestehende Zugriffe anderer Apps erhalten. Das
   GitHub-Kontopasswort ist dafür ungeeignet. Die Registry-Einrichtung erfolgt ebenfalls
   in der HA-Oberfläche.
4. **Finanzhaushalt AI → Installieren** wählen.
5. In der App unter **Konfiguration** `owner_user_id` mit der 32-stelligen HA-ID
   der Besitzerperson füllen und speichern. Die ID in **Einstellungen → Personen →
   Benutzer** beim gewünschten Benutzer nachsehen. Nicht den Anzeigenamen verwenden.
6. **Starten**, **Beim Booten starten** und **In der Seitenleiste anzeigen** aktivieren.
   Automatische Updates ausgeschaltet lassen; Updates nach einem Backup ausführen.
   **Weboberfläche öffnen**. Für KI-Imports, Chat und Diktat anschließend den OVH-Key
   in den Einstellungen der App hinterlegen und die Übermittlung aktivieren.

Bei `unauthorized`/`denied` während der Installation den Eintrag für `ghcr.io` prüfen.
Die Sichtbarkeit des Containerpakets bleibt privat.

Der Registry-Dialog ist im [offiziellen HA-Handbuch](https://www.home-assistant.io/more-info/dockerhub-rate-limit/)
beschrieben; für diese App ist der Server `ghcr.io`.

<details>
<summary>Optionale Terminal-Alternative für Administratoren</summary>

### Alternative per Terminal

Einmal diesen Block in **Terminal & SSH** oder **Advanced SSH & Web Terminal**
einfügen. Er prüft die Architektur, ergänzt nur dieses App-Repository, verwendet
vorhandene Registrydaten unverändert, installiert die App und erkennt nach Möglichkeit
die Besitzer-ID. Ein erkannter Besitzer wird vor der Freigabe angezeigt und bestätigt.
Bei bereits eingerichteter App bleiben Besitzer und Daten erhalten; dies ist kein Updater.

```bash
(
  set -eu
  finance_setup=$(mktemp)
  trap 'rm -f "$finance_setup" "$finance_setup.sh"' EXIT
  curl -fsSL https://raw.githubusercontent.com/SaschaSV/homeassistant-apps/main/finance_ai/DOCS.md -o "$finance_setup"
  sed -n '/^# FINANCE-INSTALL-BEGIN$/,/^# FINANCE-INSTALL-END$/p' "$finance_setup" > "$finance_setup.sh"
  test -s "$finance_setup.sh"
  bash "$finance_setup.sh"
)
```

Falls noch kein GHCR-Zugang eingerichtet ist, zeigt das Programm den direkten Link zur
GitHub-Tokenanlage: **PAT classic**, Berechtigung **read:packages**, Benutzer **SaschaSV**.
Token nur in der verdeckten Terminalabfrage eingeben. Er wird im Supervisor gespeichert;
kein API-Key oder Token gehört in Chat, Shellhistorie oder App-Konfiguration. Ein bereits
vorhandener Registryeintrag wird nicht überschrieben. Bei dessen fehlendem Paketrecht
zuerst den vorhandenen Zugang passend erweitern; andere private Apps müssen weiter funktionieren.

Danach „Finanzhaushalt AI“ in der Seitenleiste öffnen. Für manuelle Nutzung sind Bank und
KI optional. Für unbekannte Dateien, KI-Chat und Diktat den eigenen OVH-Key in der App
hinterlegen und die entsprechende Übermittlung aktivieren. Die Bank kommt anschließend hinzu.

Die folgenden Befehle sind die vollständige, überprüfbare Installationsanleitung,
die der obige Block aus dieser Datei entnimmt. Sie ändern keine anderen Apps,
setzen keine Passwörter zurück und führen kein System-/App-Update aus.

<details>
<summary>Installationsbefehle ansehen</summary>

```bash
# FINANCE-INSTALL-BEGIN
# Geführte Ersteinrichtung im Home-Assistant-Terminal, kein Update vorhandener Daten.
set +x
set -euo pipefail
umask 077
for finance_command in curl jq mktemp; do
  command -v "$finance_command" >/dev/null || { echo "$finance_command fehlt im Terminal." >&2; exit 1; }
done
[[ ${SUPERVISOR_TOKEN:-} =~ ^[A-Za-z0-9._-]+$ ]] || {
  echo 'Bitte in Terminal & SSH / Advanced SSH & Web Terminal innerhalb von Home Assistant ausführen.' >&2
  exit 1
}
finance_tmpdir=$(mktemp -d)
trap 'rm -rf "$finance_tmpdir"' EXIT
printf 'header = "Authorization: Bearer %s"\nheader = "Content-Type: application/json"\n' "$SUPERVISOR_TOKEN" > "$finance_tmpdir/auth"
finance_api() {
  local finance_method=$1 finance_path=$2 finance_code
  local finance_args=()
  if [[ $# == 3 ]]; then finance_args=(--data-binary "@$3"); fi
  finance_code=$(curl --silent --show-error --connect-timeout 15 --max-time 1200 \
    --config "$finance_tmpdir/auth" --request "$finance_method" \
    --output "$finance_tmpdir/response" --write-out '%{http_code}' \
    "${finance_args[@]}" "http://supervisor$finance_path") || {
      echo "Supervisor nicht erreichbar: $finance_path. Erneut ausführen ist möglich." >&2; return 1;
    }
  if [[ ! $finance_code =~ ^2[0-9][0-9]$ ]] || ! jq -e '.result == "ok"' "$finance_tmpdir/response" >/dev/null; then
    echo "Schritt $finance_path fehlgeschlagen (HTTP $finance_code)." >&2
    echo 'Bei 401/403 Terminal-Berechtigung bzw. bestehenden GHCR-Lesezugang prüfen. Keine Tokens in Fehlermeldungen weitergeben.' >&2
    return 1
  fi
}
finance_api GET /info
finance_arch=$(jq -r '.data.arch // "unbekannt"' "$finance_tmpdir/response")
[[ $finance_arch == aarch64 || $finance_arch == amd64 ]] || {
  echo "Benötigt 64-Bit-HAOS (aarch64/amd64); erkannt: $finance_arch. Keine Installation vorgenommen." >&2; exit 1;
}
finance_repo='https://github.com/SaschaSV/homeassistant-apps'
finance_api GET /store
finance_find_repo() {
  jq -r --arg repo "$finance_repo" '[.data.repositories[] | select(.source == $repo or .url == $repo) | .slug] | unique | if length == 1 then .[0] else "" end' "$finance_tmpdir/response"
}
finance_repository=$(finance_find_repo)
if [[ -z $finance_repository ]]; then
  jq -n --arg repository "$finance_repo" '{repository:$repository}' > "$finance_tmpdir/body"
  finance_api POST /store/repositories "$finance_tmpdir/body"
fi
echo 'App-Katalog aktualisieren …'
finance_api POST /store/reload
finance_api GET /store
finance_repository=$(finance_find_repo)
[[ $finance_repository =~ ^[a-z0-9]{8,32}$ ]] || { echo 'Das erwartete App-Repository wurde nicht eindeutig gefunden.' >&2; exit 1; }
finance_slug="${finance_repository}_finance_ai"
jq -e --arg slug "$finance_slug" '.data.addons[] | select(.slug == $slug and .available == true)' "$finance_tmpdir/response" >/dev/null || {
  echo 'Finanzhaushalt AI ist noch nicht verfügbar. HA/HAOS-Version und den veröffentlichten Katalog prüfen.' >&2; exit 1;
}
finance_installed=$(jq -r --arg slug "$finance_slug" '.data.addons[] | select(.slug == $slug) | .installed' "$finance_tmpdir/response")
if [[ $finance_installed != true ]]; then
  finance_api GET /docker/registries
  if jq -e '.data.registries["ghcr.io"] != null' "$finance_tmpdir/response" >/dev/null; then
    echo 'Vorhandener ghcr.io-Zugang bleibt erhalten und wird verwendet.'
  else
    echo 'GitHub-Lesetoken (classic, nur read:packages) für SaschaSV anlegen:'
    echo 'https://github.com/settings/tokens/new?description=Home%20Assistant%20Finance%20AI&scopes=read:packages'
    echo 'Den Token ausschließlich hier verdeckt eingeben; nicht im Chat.'
    read -r -s -p 'GitHub-Lesetoken: ' finance_pat
    printf '\n'
    [[ $finance_pat =~ ^ghp_[A-Za-z0-9]{20,255}$ ]] || { echo 'Kein gültiger PAT classic. Abgebrochen.' >&2; exit 1; }
    printf '%s' "$finance_pat" > "$finance_tmpdir/token"
    unset finance_pat
    jq -n --rawfile token "$finance_tmpdir/token" '{"ghcr.io":{username:"SaschaSV",password:$token}}' > "$finance_tmpdir/body"
    finance_api POST /docker/registries "$finance_tmpdir/body"
    rm -f "$finance_tmpdir/token" "$finance_tmpdir/body"
  fi
  echo 'Privates App-Image herunterladen und installieren; das kann einige Minuten dauern …'
  finance_api POST "/store/addons/$finance_slug/install"
fi
finance_api GET "/addons/$finance_slug/info"
cp "$finance_tmpdir/response" "$finance_tmpdir/app"
finance_previous_owner=$(jq -r '.data.options.owner_user_id // ""' "$finance_tmpdir/app")
finance_owner=$finance_previous_owner
if [[ ! $finance_owner =~ ^[a-f0-9]{32}$ ]]; then
  for finance_auth_file in /homeassistant/.storage/auth /config/.storage/auth; do
    if [[ -r $finance_auth_file ]]; then
      finance_owner=$(jq -r '[.data.users[] | select(.is_owner == true and .is_active != false) | .id] | if length == 1 then .[0] else "" end' "$finance_auth_file")
      [[ $finance_owner =~ ^[a-f0-9]{32}$ ]] && break
    fi
  done
  if [[ $finance_owner =~ ^[a-f0-9]{32}$ ]]; then
    printf 'HA-Besitzer erkannt: %s\n' "$finance_owner"
    read -r -p 'Enter = diesen Besitzer verwenden; alternativ andere HA-Nutzer-ID eingeben: ' finance_choice
    finance_owner=${finance_choice:-$finance_owner}
  else
    echo 'Unter Einstellungen → Personen → Benutzer die 32-stellige ID des gewünschten Besitzers kopieren.'
    read -r -p 'Besitzer-HA-Nutzer-ID: ' finance_owner
  fi
fi
[[ $finance_owner =~ ^[a-f0-9]{32}$ ]] || { echo 'Ungültige HA-ID. Noch kein Zugriff freigeschaltet.' >&2; exit 1; }
jq --arg owner "$finance_owner" '{options:((.data.options // {}) + {owner_user_id:$owner}),boot:"auto",auto_update:false,ingress_panel:true}' "$finance_tmpdir/app" > "$finance_tmpdir/body"
finance_api POST "/addons/$finance_slug/options" "$finance_tmpdir/body"
finance_state=$(jq -r '.data.state' "$finance_tmpdir/app")
if [[ $finance_state == started && $finance_previous_owner != "$finance_owner" ]]; then
  finance_api POST "/addons/$finance_slug/restart"
elif [[ $finance_state != started ]]; then
  finance_api POST "/addons/$finance_slug/start"
fi
finance_api GET "/addons/$finance_slug/info"
[[ $(jq -r '.data.state' "$finance_tmpdir/response") == started ]] || {
  echo 'Installiert, Start noch nicht bestätigt. App-Info und Protokoll in Home Assistant prüfen.' >&2; exit 1;
}
echo 'Finanzhaushalt AI ist installiert und gestartet. Jetzt in der Seitenleiste öffnen:'
printf 'https://my.home-assistant.io/redirect/supervisor_addon/?addon=%s\n' "$finance_slug"
echo 'Manuelle Ausgaben und bekannte Importe sind sofort nutzbar. KI/OVH-Key unter Einstellungen aktivieren.'
echo 'App in den verschlüsselten HA-Backupplan aufnehmen. Spätere Updates mit vorgeschaltetem Backup ausführen.'
# FINANCE-INSTALL-END

```

</details>


</details>

## Manuelle Einrichtung und Bedienung

1. Ein berechtigter Administrator installiert das private Image über den bestehenden
   Katalog `https://github.com/SaschaSV/homeassistant-apps`. Zuerst GHCR-Lesezugang
   im Supervisor setzen. Keinen öffentlichen Imagezugriff aktivieren.
2. Unter App-Konfiguration `owner_user_id` auf die 32-stellige HA-Nutzer-ID setzen.
   ID unter **Einstellungen → Personen → Benutzer** prüfen. App starten und über
   **Weboberfläche öffnen** aufrufen. Fehlende Identität führt zu einer Zugriffssperre.
3. Eine erste Ausgabe benötigt Betrag und Beschreibung. Profile heißen zunächst
   „Ich“ und „Saskia“; unter Einstellungen umbenennen. Eine zweite HA-ID gezielt zulassen.
   Jede berechtigte Person sieht ausschließlich ihr eigenes Profil.
4. Optional unter **KI & Sprache** die Übermittlung notwendiger Auszüge an OVHcloud
   aktivieren, Key hinterlegen, Modellkatalog und Verbindung testen. Standard Qwen3.8-27B/xhigh
   bleibt erhalten. Keine API-Schlüssel im Chat eingeben.
5. Direktes Diktat: sichere HTTPS-HA-Adresse, Mikrofonberechtigung im Browser und in
   Android zulassen. Im Chat aufnehmen, stoppen, Transkript prüfen/bearbeiten, senden.
   Unter gewöhnlichem HTTP ist nur das ausdrücklich benannte Tastaturdiktat möglich.
   Bei gesperrtem Mikrofon Ingress-/iframe-Permissions-Policy und Companion-App prüfen.
   Es wird keine Assist-Pipeline verändert.
6. Optional Sparkasse einrichten: eigene FinTS-Produkt-ID registrieren, Anmeldename/PIN
   ausschließlich im geschützten Bankformular eingeben, angebotene pushTAN-Methode wählen,
   Konten erkennen und vorhandenen App-Konten zuordnen. Erst danach den täglichen Abruf
   aktivieren. Freigaben erscheinen unter „Noch zu klären“. PIN-Fehler lösen keine aggressiven
   Wiederholungen aus. Nach App-Neustart/Restore wird eine neue Bankfreigabe benötigt.

## Datenbereiche

**Planung** rechnet Annahmen auf Monatsdurchschnitte um. Jahreshochrechnung und
Kalenderjahr sind getrennt. Einmalpositionen benötigen einen Monat, Preisänderungen
einen Gültigkeitsmonat. Planpositionen erzeugen keine echten Buchungen.

**Haushaltsbuch** enthält echte Einnahmen, Ausgaben, Transfers und Erstattungen.
Gemeinsame Ausgaben werden einmal erfasst und aufgeteilt. Konto und wirtschaftliche
Anteile sind unabhängig. Transfers, Kreditkartenausgleich und eigene PayPal-Einzüge
werden nach eindeutiger Zuordnung nicht als zusätzlicher Konsum gezählt. Bei einer
Erstattung kann die ursprüngliche Ausgabe referenziert werden.

**Vermögen** verwendet je Konto den letzten Stand bis zum gewählten Stichtag.
Fehlende Stände sind unbekannt, ältere Stände sichtbar. Verbindlichkeiten positiv
eingeben; negative Girostände als negative Vermögenswerte. Bestandsverlauf ist keine Rendite.

## Import, Export und Korrektur

CSV/TSV, ODS und XLSX oder kopierte Tabelle unter **Daten** hinzufügen. Das aktuelle
Profil und ausgewählte Konto sind Kontext. Bekannte Zuordnungen laufen lokal; unbekannte
Strukturen werden bei aktivierter Importhilfe mit Qwen interpretiert. Geprüfte Zeilen
werden sofort übernommen; ausschließlich unklare Zeilen werden gebündelt zurückgestellt.
Formeln werden nicht ausgeführt. Bei fehlenden Ergebnissen in Calc/Excel neu berechnen
und speichern oder CSV verwenden. Kein stiller Fremdwährungsimport.

Grenzen: 10 MiB, 50.000 Zeilen, 100 Spalten, 500.000 materialisierte Zellen;
ein schwerer Import gleichzeitig. Dateien können abgebrochen und wiederaufgenommen
werden. Bereits abgeschlossene Teilbatches bleiben nachvollziehbar. Gleiche Datei im
gleichen Kontext erzeugt keinen zweiten Import. Mögliche Dubletten erfordern eine Auswahl.

Tabellen direkt bearbeiten oder markieren und im Chat ansprechen. Neue eindeutige
Einzelaufträge werden sofort gespeichert; umfangreiche Änderungen, Regeln und Planung
werden gemeinsam vorgeschaut. Rückgängig respektiert spätere Bearbeitungen.

CSV/ODS-Export übernimmt Profil, Auswahl und Filter; Gesamtbestand ist separat auswählbar.
Versionierter ZIP-Datenexport mit Wiederimport enthält keine Schlüssel/PIN/Bankdialoge.
Rohdateien standardmäßig sieben Tage aufbewahren; normalisierte Herkunft bleibt erhalten.
Kein PDF-Bericht. Keine Demodaten im echten Bestand.

## Backup und Updates

Finanzhaushalt AI in den verschlüsselten HA-Backupplan aufnehmen; fertige Backups mit
einer bewusst gewählten Anbindung extern sichern. Recovery Kit außerhalb des Pi aufbewahren.
`backup: cold` stoppt die App für konsistente Sicherung aller Daten einschließlich Secret-Key.
Es erstellt keinen eigenen Zeitplan und erzwingt nicht von selbst ein Backup vor Updates.

Den bereitgestellten HA-Ablauf mit `update.install` und `backup: true` bewusst einrichten;
native Auto-Updates deaktivieren, wenn sie ihn umgehen. Ein fehlgeschlagenes Backup muss
das Update auf der Zielinstallation blockieren. Die App erhält dafür keine Supervisorrechte.
Vor Migrationen entstehen zusätzlich bis zu fünf konsistente lokale Datenbank-Sicherungen.
Ein Downgrade auf eine inkompatible Datenbankversion stoppt; passendes Image und vollständiges
Backup gemeinsam wiederherstellen. Keine leere Ersatzdatenbank bei Lesefehlern.

## Grenzen und Datenschutz

Alle fachlichen Daten liegen lokal unter `/data`. Aktivierte KI sendet erforderliche
Auszüge an OVHcloud; Sprachaufnahme nur nach Mikrofondruck, keine dauerhafte Aufnahme.
Keine externen Fonts, CDNs oder Telemetrie. Berechtigte Imageempfänger und Hostadministratoren
können ausgelieferten Laufzeitcode und lokale Daten untersuchen. Die lokale Verschlüsselung
ist keine Schutzgrenze gegen den Hostadministrator.

Der persönliche Sparkassen-Zugang, HAOS-Update/Restore, privater Registry-Pull und das echte
Android-Mikrofon müssen bei der gesondert beauftragten Einrichtung abgenommen werden.

## Update 0.2.0: private Profile und Jahresbudget

Vor Update ein vollständiges HA-Backup erstellen. Jede HA-Anmeldung sieht anschließend
nur ihr eigenes Profil. Gemeinsame Altbeträge werden nach gespeicherten Anteilen
centgenau aufgeteilt; das vollständige Altarchiv bleibt geschützt erhalten. Alte Chats,
Rohimporte und Bankzugänge werden nicht verteilt. Bankzugänge je Person neu verbinden;
Saskia hinterlegt ihre eigene KI-Verbindung. Ihre HA-ID unter „Profil & Zugriff“ einmalig
zuordnen; eine bestehende Zuordnung kann gesperrt, aber nicht auf eine fremde ID umgebunden werden.

Planung enthält monatliche/jährliche Einnahmen und Ausgaben als Schätzung. Jahreskosten
werden auf zwölf Monate umgelegt. ODS-/XLSX-/CSV-Budgettabellen direkt dort importieren.
Bei pausiertem Import die sichtbare Meldung beachten und später fortsetzen.
Rollback ausschließlich mit passendem vollständigem Backup, nicht nur altem Image.
