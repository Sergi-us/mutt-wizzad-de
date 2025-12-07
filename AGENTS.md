# mutt-wizard Agent-Richtlinien

## Build/Test-Befehle
- **Hauptinstallation**: `sudo make install` (installiert nach /usr/local)
- **Installation testen**: `mw -h` (zeigt Hilfe an)
- **Account hinzufügen**: `mw -a your@email.com`
- **Accounts auflisten**: `mw -l`
- **Account löschen**: `mw -d`
- **Mailsync testen**: `mailsync` (synchronisiert alle konfigurierten Accounts)
- **Konfiguration prüfen**: 
  - `cat ~/.config/mutt/muttrc` (Haupt-Mutt-Konfiguration)
  - `cat ~/.mbsyncrc` (mbsync/isync Konfiguration)
  - `cat ~/.config/msmtp/config` (SMTP Konfiguration)
- **domains.csv validieren**: Syntax mit `head -5 share/domains.csv` überprüfen und korrekte Komma-Trennung sicherstellen

## Code-Style-Richtlinien

### Shell-Script-Style
- `#!/bin/bash` Shebang für Bash-Kompatibilität verwenden (Projekt nutzt Bash-spezifische Features)
- Bestehende Funktionsbenennung befolgen: Kleinbuchstaben mit beschreibenden Namen (`checkbasics`, `getaccounts`, `prepmutt`)
- Deutsche Kommentare und benutzerseitige Strings für deutsche Version verwenden
- Alle Variablen quoten: `"$fulladdr"`, `"$imap"`, `"$muttrc"`
- `>/dev/null 2>&1` zum Unterdrücken von Befehlsausgaben verwenden
- Fehlerbehandlung: `|| { echo "Fehlermeldung" && exit 1; }` Muster verwenden
- Einrückung: Tabs verwenden (entsprechend der bestehenden Codebase)
- `set -a` für automatisches Exportieren von Variablen (für envsubst)

### Templates und Konfigurationsdateien
- Templates in `share/` verwenden envsubst für Variablenersetzung
- Template-Dateien: `*-temp` Suffix (z.B. `mutt-temp`, `mbsync-temp`, `msmtp-temp`)
- XDG Base Directory Specification einhalten:
  - Config: `${XDG_CONFIG_HOME:-$HOME/.config}`
  - Data: `${XDG_DATA_HOME:-$HOME/.local/share}`
  - Cache: `${XDG_CACHE_HOME:-$HOME/.cache}`
  - State: `${XDG_STATE_HOME:-$HOME/.local/state}`

### Abhängigkeiten
- **Erforderlich**: `neomutt`, `curl`, `isync`/`mbsync`, `msmtp`, `pass`, `ca-certificates`, `gettext`
- **Optional**: `goimapnotify`, `pam-gnupg`, `lynx`, `notmuch`, `abook`, `urlview`, `cronie`, `mpop`
- GPG wird für pass-Integration benötigt
- Vor Installation prüfen: `command -V <command> >/dev/null 2>&1`

### Benennungskonventionen
- Variablen: Kleinbuchstaben ohne Unterstriche (`fulladdr`, `imap`, `smtp`, `maildir`)
- Funktionen: Kleinbuchstaben, beschreibend, Verb-zuerst (`checkbasics`, `getaccounts`, `parsedomains`, `prepmutt`)
- Globale Pfade: Lowercase mit Suffix (`muttrc`, `mbsyncrc`, `msmtprc`, `accdir`)
- Template-Pfade: Mit `-temp` Suffix (`mutttemp`, `mbsynctemp`, `msmtptemp`)
- Account-ID: `idnum` für numerische Account-Identifikation (1-9)
- Sichere Namen: `safename` für Dateinamen ohne @ (z.B. für Cache-Verzeichnisse)

### Fehlerbehandlung
- Email-Validierung: `grep -qE "^.+@.+\.[A-Za-z]+$"` Muster
- Passwort-Store prüfen: `[ -r "$PASSWORD_STORE_DIR/.gpg-id" ]`
- SSL-Zertifikat-Validierung: Mehrere Standard-Pfade durchsuchen
- Provider-spezifische Fehlermeldungen für Gmail/iCloud
- Curl mit Timeout: `curl -s -m 5` für Server-Verbindungen
- Exit Codes: `exit 1` für Fehler, `return 0` für Erfolg

### Datei-Organisation
- **bin/**: Ausführbare Skripte (`mw`, `mailsync`)
- **share/**: Template-Dateien, globale Konfigurationen, domains.csv
- **completion/**: Shell-Completion-Skripte
- **lib/**: Helper-Skripte (z.B. `openfile`)
- Makefile für Installation verwenden
- Man-Pages: `mw.1`, `mailsync.1`

### Email-Account-Verwaltung
- Accounts in `~/.config/mutt/accounts/` als separate `.muttrc` Dateien
- Account-Nummerierung: Makros `i1` bis `i9` für Account-Wechsel
- Passwörter in pass-Store: `mw-your@email.com` oder mit Prefix
- mbsync-Versionen: Far/Near (>=1.4.0) vs. Master/Slave (<1.4.0) beachten
- Drei Account-Typen unterstützen:
  1. Standard IMAP (offline mit mbsync)
  2. Online IMAP (keine lokale Kopie)
  3. POP3 (mit mpop)

### domains.csv Format
- Format: `domain,imap-server,imap-port,smtp-server,smtp-port`
- Wildcard-Unterstützung: `.*\.domain\.com` für Subdomains
- Beliebte Provider vorkonfigurieren (Gmail, Outlook, Yahoo, etc.)
- Komma als Trennzeichen, keine Leerzeichen

### Sicherheit
- Passwörter nur via `pass` speichern, nie im Klartext
- GPG-verschlüsselte Passwortspeicherung
- SSL/TLS für alle Verbindungen
- CA-Zertifikat-Validierung erforderlich
- Option für App-spezifische Passwörter (Gmail, iCloud)

### Internationalisierung
- Deutsche Übersetzung für Benutzer-Nachrichten
- Englische Mailbox-Namen bevorzugen (INBOX, Sent, Drafts, etc.)
- UTF-8 Limitierungen von isync beachten
- Hinweis: isync hat Probleme mit nicht-lateinischen Zeichen

## Besondere Funktionen
- **envsubst**: Wird für Template-Substitution verwendet (aus gettext)
- **pass**: Password-Store-Integration für sichere Passwort-Verwaltung
- **notmuch**: Optionale Mail-Indizierung und Suche
- **goimapnotify**: Push-Benachrichtigungen für neue Mails
- **mailsync**: Separates Skript für Mail-Synchronisation und Benachrichtigungen
- **Account-Reordering**: `mw -r` zum Neuordnen der Account-Nummern

## Wichtige Konfigurationsdateien
- `~/.config/mutt/muttrc`: Haupt-Mutt-Konfiguration
- `~/.config/mutt/accounts/*.muttrc`: Account-spezifische Konfigurationen
- `~/.mbsyncrc`: isync/mbsync Konfiguration für IMAP
- `~/.config/msmtp/config`: SMTP-Konfiguration für Mailversand
- `~/.config/mpop/config`: POP3-Konfiguration (optional)
- `~/.config/imapnotify/*.conf`: goimapnotify Konfiguration pro Account
- `~/.notmuch-config`: notmuch-Konfiguration (optional)
- `/usr/local/share/mutt-wizard/mutt-wizard.muttrc`: Globale Mutt-Einstellungen

## Cronjob-Integration
- `mw -t <Minuten>`: Automatische Mailsynchronisation aktivieren/deaktivieren
- Standard: Alle 10 Minuten
- Verwendet system crontab
- Ruft `mailsync` auf

## Testing-Hinweise
- In VM oder Container testen (verändert Benutzerkonfiguration)
- GPG-Schlüssel muss vor Tests existieren
- Pass-Store muss initialisiert sein: `pass init <gpg-email>`
- Test-Email-Account bereithalten
- Firewall-Regeln für IMAP/SMTP (Ports 993, 465, 587, 143) prüfen
