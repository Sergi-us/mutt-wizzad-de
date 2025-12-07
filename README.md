# 📧 mutt-wizard - E-Mail-Setup leicht gemacht

**Automatisierte Konfiguration von Neomutt mit IMAP/POP3 und SMTP**

> **🔄 Umzug zu Codeberg**: Die deutsche Version und aktive Entwicklung findet auf [Codeberg](https://codeberg.org/Sergius/mutt-wizzard-de) statt. GitHub dient als Mirror.
> 
> **🌐 Original**: [muttwizard.com](https://muttwizard.com/) von Luke Smith

Ein intelligentes Shell-Skript, das automatisch vollständig konfigurierte E-Mail-Konten für Neomutt einrichtet - mit Offline-Speicherung, Verschlüsselung und Push-Benachrichtigungen.

## 🎯 Was ist mutt-wizard?

mutt-wizard (`mw`) richtet automatisch ein:

- ✅ **Neomutt** - Vollständig konfigurierter Terminal-E-Mail-Client
- ✅ **mbsync** - IMAP-Synchronisation für Offline-Zugriff
- ✅ **msmtp** - SMTP zum Versenden von E-Mails
- ✅ **pass** - Verschlüsselte Passwortspeicherung via GPG
- ✅ **mailsync** - Automatische Synchronisation mit Benachrichtigungen
- ✅ **Mehrere Konten** - Bis zu 9 E-Mail-Konten gleichzeitig

### Hauptfeatures

- 📥 **Offline-Zugriff** - E-Mails lokal gespeichert und durchsuchbar
- 🔐 **Sicher** - Passwörter GPG-verschlüsselt mit `pass`
- 🔔 **Benachrichtigungen** - Push-Benachrichtigungen für neue E-Mails (optional)
- ⚡ **Automatisch** - Server-Einstellungen für bekannte Provider vorkonfiguriert
- 🎨 **Ansprechend** - Sinnvolle Defaults und attraktive Darstellung
- ⌨️ **Vim-Bindings** - Effiziente Navigation im Terminal

## ⚡ Installation

### Abhängigkeiten

**Erforderlich:**
```bash
# Arch Linux
sudo pacman -S neomutt isync msmtp pass curl ca-certificates gettext

# Debian/Ubuntu
sudo apt install neomutt isync msmtp pass curl ca-certificates gettext-base
```

**Optional (empfohlen):**
```bash
# Arch Linux
sudo pacman -S goimapnotify lynx notmuch abook urlview cronie mpop

# Debian/Ubuntu  
sudo apt install goimapnotify lynx notmuch abook urlview cron mpop
```

**Hinweis:** Auf langsamen Release-Distributionen (Ubuntu, Debian, Mint) kann eine veraltete neomutt-Version Probleme verursachen. Entweder manuell die neueste Version installieren oder fehlerhafte Zeilen in `/usr/share/mutt-wizard/mutt-wizard.muttrc` entfernen.

### Installation von mutt-wizard

```bash
git clone https://codeberg.org/Sergius/mutt-wizzard-de
cd mutt-wizzard-de
sudo make install
```

**Arch Linux AUR:**
```bash
# Stable Release
yay -S mutt-wizard

# Git Master Branch
yay -S mutt-wizard-git
```

## 🚀 Schnellstart

### 1. GPG-Schlüssel einrichten

Falls noch nicht vorhanden:
```bash
gpg --full-generate-key
```

### 2. Pass initialisieren

```bash
pass init deine-gpg-email@example.com
```

### 3. E-Mail-Konto hinzufügen

```bash
mw -a deine@email.com
```

mutt-wizard erkennt automatisch die Server-Einstellungen für bekannte Provider (Gmail, Outlook, Yahoo, etc.). Falls nicht bekannt, wirst du nach IMAP/SMTP-Details gefragt.

### 4. E-Mails abrufen

```bash
mailsync
```

### 5. Neomutt starten

```bash
neomutt
```

## 📖 Verwendung

### Grundbefehle

| Befehl | Beschreibung |
|--------|--------------|
| `mw -a email@example.com` | E-Mail-Konto hinzufügen |
| `mw -l` | Alle konfigurierten Konten auflisten |
| `mw -d` | Konto löschen (interaktiv) |
| `mw -D email@example.com` | Konto ohne Bestätigung löschen |
| `mw -t 30` | Auto-Sync alle 30 Minuten aktivieren |
| `mw -T` | Auto-Sync mit Standard (10 Min) aktivieren |
| `mw -r` | Konto-Nummern neu ordnen |
| `mailsync` | Alle Konten synchronisieren |
| `mailsync email@example.com` | Bestimmtes Konto synchronisieren |
| `pass edit mw-email@example.com` | Passwort ändern |

### Optionen beim Hinzufügen eines Kontos

```bash
mw -a email@example.com \
   -u username \              # Login-Name (falls abweichend)
   -n "Echter Name" \         # Anzeigename
   -i imap.server.com \       # IMAP-Server
   -I 993 \                   # IMAP-Port (Standard: 993)
   -s smtp.server.com \       # SMTP-Server
   -S 465 \                   # SMTP-Port (Standard: 465)
   -m 5000 \                  # Max. Anzahl Offline-E-Mails
   -x "passwort" \            # Passwort direkt angeben
   -p \                       # POP3 statt IMAP verwenden
   -o \                       # Online-Modus (kein Offline-Speicher)
   -f                         # Standard-Mailboxen annehmen
```

### Neomutt Tastenkombinationen

Die wichtigsten Befehle in Neomutt:

| Taste | Funktion |
|-------|----------|
| <kbd>m</kbd> | Neue E-Mail schreiben |
| <kbd>j</kbd>/<kbd>k</kbd> | Hoch/Runter navigieren |
| <kbd>d</kbd>/<kbd>u</kbd> | Seite runter/hoch |
| <kbd>l</kbd> | E-Mail/Anhang öffnen |
| <kbd>h</kbd> | Zurück |
| <kbd>r</kbd>/<kbd>R</kbd> | Antworten/Allen antworten |
| <kbd>s</kbd> | E-Mail/Anhang speichern |
| <kbd>gs</kbd>, <kbd>gi</kbd>, <kbd>ga</kbd>, <kbd>gd</kbd>, <kbd>gS</kbd> | Zu Sent/Inbox/Archive/Drafts/Spam |
| <kbd>M</kbd>, <kbd>C</kbd> | Verschieben/Kopieren (+ Mailbox-Buchstabe) |
| <kbd>i1</kbd>-<kbd>i9</kbd> | Zu Konto 1-9 wechseln |
| <kbd>a</kbd> | Adresse zu abook hinzufügen |
| <kbd>Tab</kbd> | Adresse aus abook vervollständigen |
| <kbd>?</kbd> | Alle Tastenkombinationen anzeigen |
| <kbd>Ctrl+j</kbd>/<kbd>Ctrl+k</kbd> | In Sidebar hoch/runter |
| <kbd>Ctrl+o</kbd> | Mailbox aus Sidebar öffnen |
| <kbd>Ctrl+b</kbd> | URL-Menü öffnen |
| <kbd>Ctrl+f</kbd> | Notmuch-Suche |
| <kbd>p</kbd> | E-Mail verschlüsseln/signieren |

## 🔧 Erweiterte Konfiguration

### Push-Benachrichtigungen aktivieren

Für sofortige Benachrichtigungen bei neuen E-Mails:

```bash
systemctl --user enable goimapnotify@deine@email.com.service
systemctl --user start goimapnotify@deine@email.com.service
```

**Hinweis:** Ersetze `deine@email.com` mit deiner tatsächlichen E-Mail-Adresse (inkl. `@`-Zeichen).

### Automatische Synchronisation

```bash
# Alle 10 Minuten (Standard)
mw -T

# Alle 30 Minuten
mw -t 30

# Deaktivieren
mw -T  # Toggle schaltet auch aus
```

Dies richtet einen Cronjob ein, der `mailsync` regelmäßig ausführt.

### Notmuch für E-Mail-Suche

```bash
# Installation (falls noch nicht installiert)
sudo pacman -S notmuch

# Setup
notmuch setup
# Maildir: ~/.local/share/mail/

# Neue E-Mails indizieren
notmuch new

# In Neomutt: Ctrl+f für Suche
```

mutt-wizard konfiguriert notmuch automatisch, falls es installiert ist.

### GPG Auto-Unlock mit pam-gnupg

Für automatisches Entsperren des GPG-Schlüssels beim Login:

```bash
# Arch Linux
yay -S pam-gnupg

# Anleitung: https://github.com/cruegge/pam-gnupg
```

## 📁 Wichtige Dateien & Verzeichnisse

```
~/.config/mutt/
├── muttrc                          # Haupt-Konfiguration
└── accounts/
    ├── email1@example.com.muttrc   # Konto-spezifische Config
    └── email2@example.com.muttrc

~/.local/share/mail/                # Offline-E-Mail-Speicher
├── email1@example.com/
│   ├── INBOX/
│   ├── Sent/
│   ├── Drafts/
│   └── ...
└── email2@example.com/

~/.mbsyncrc                         # mbsync (IMAP) Konfiguration
~/.config/msmtp/config              # SMTP Konfiguration
~/.config/mpop/config               # POP3 Konfiguration (optional)
~/.config/imapnotify/               # Push-Benachrichtigungs-Configs
~/.notmuch-config                   # Notmuch Suchindex-Config
~/.password-store/                  # GPG-verschlüsselte Passwörter

/usr/local/share/mutt-wizard/
├── mutt-wizard.muttrc              # Globale Mutt-Einstellungen
├── domains.csv                     # Bekannte E-Mail-Provider
└── *-temp                          # Config-Templates
```

## 🔐 Sicherheit & Passwörter

### Passwort ändern

```bash
pass edit deine@email.de
```

### Passwort-Präfix verwenden

Falls du mehrere Pass-Archive nutzt:

```bash
mw -a email@example.com -P "arbeit/"
# Passwort wird in: arbeit/email@example.com gespeichert
```

### Gmail & App-Passwörter

Gmail (und andere Google-Dienste) benötigen ein **App-Passwort**:

1. Google-Konto > Sicherheit > [App-Passwörter](https://myaccount.google.com/apppasswords)
2. Neues App-Passwort für "Mail" erstellen
3. Dieses Passwort bei `mw -a` Eingabe verwenden

**Hinweis:** IMAP muss in Gmail-Einstellungen aktiviert sein.

### iCloud & App-Passwörter

Ähnlich wie Gmail benötigt iCloud ein App-spezifisches Passwort:

1. Apple ID > Sicherheit > App-spezifische Passwörter
2. Neues Passwort generieren
3. Bei `mw -a` verwenden

## ⚠️ Bekannte Einschränkungen

### UTF-8 & Nicht-lateinische Zeichen

`mbsync` (ehemals `isync`) hat Probleme mit nicht-lateinischen Zeichen in Mailbox-Namen. **Empfehlung:** E-Mail-Sprache auf Englisch stellen für:
- INBOX, Sent, Drafts, Trash, etc.

Sonst können Mailbox-Shortcuts in Neomutt nicht automatisch erstellt werden.

### Unternehmens-E-Mail & 2FA

Universitäts- oder Firmen-E-Mails haben oft zusätzliche Sicherheitsmaßnahmen:
- Separate IMAP-Passwörter
- OAuth-Authentifizierung
- Spezielle Proxy-Einstellungen

Konsultiere deine IT-Abteilung für IMAP/SMTP-Details.

### Langsame Distributionen

Ubuntu, Debian, Mint haben oft veraltete Neomutt-Versionen. Bei Fehlern:
1. Neueste Neomutt-Version manuell installieren, **oder**
2. Fehlerhafte Zeilen in `/usr/local/share/mutt-wizard/mutt-wizard.muttrc` entfernen

## 🎨 Anpassung

### Eigene Mutt-Einstellungen

In `~/.config/mutt/muttrc` kannst du beliebige Einstellungen hinzufügen:

```muttrc
# Am Ende der Datei eigene Settings:
set editor = "nvim"
set date_format = "%d.%m.%Y %H:%M"
color index brightblue default "~N"  # Neue Mails blau
```

**Wichtig:** Die von mutt-wizard generierten Zeilen nicht löschen (besonders `source`-Befehle).

### Provider zu domains.csv hinzufügen

Falls dein E-Mail-Provider nicht automatisch erkannt wird:

```bash
sudo nano /usr/local/share/mutt-wizard/domains.csv
```

Format: `domain,imap-server,imap-port,smtp-server,smtp-port`

Beispiel:
```csv
example.com,imap.example.com,993,smtp.example.com,465
```

Für Subdomains Wildcards verwenden:
```csv
.*\.example\.com,imap.example.com,993,smtp.example.com,465
```

**Pull Requests willkommen!** Füge deinen Provider hinzu und teile ihn.

### Konto-spezifische Einstellungen

Für Konto-spezifische Anpassungen:

```bash
nano ~/.config/mutt/accounts/deine@email.com.muttrc
```

Diese Datei wird beim Wechsel zum Konto geladen.

## 📚 Unterschiede zum Original

Diese deutsche Version basiert auf Luke Smith's [mutt-wizard](https://github.com/LukeSmithxyz/mutt-wizard) mit folgenden Anpassungen:

- 🇩🇪 Deutsche Übersetzung aller Ausgaben und Kommentare
- 📖 Deutsche Dokumentation im SARBS-Stil
- 🔄 Aktualisierte Abhängigkeiten und Best Practices
- ⚙️ Angepasst für deutsche Nutzungsgewohnheiten

### Technische Verbesserungen gegenüber älteren Versionen

- ✅ `mbsync`/`isync` statt veraltetem `offlineimap`
- ✅ `pass` als Passwort-Manager (statt Klartext)
- ✅ XDG Base Directory Spezifikation
- ✅ Sauberere Verzeichnisstruktur
- ✅ `dialog` entfernt (nur noch Text-Interface)
- ✅ POSIX-Shell-kompatibel
- ✅ POP3-Unterstützung via `mpop`
- ✅ Besseres Attachment-Handling
- ✅ abook-Integration standardmäßig

## 🤝 Beitragen

### Bug Reports & Feature Requests

- **[Codeberg Issues](https://codeberg.org/Sergius/mutt-wizzard-de/issues)** - Für die deutsche Version
- **[GitHub Issues](https://github.com/LukeSmithxyz/mutt-wizard/issues)** - Für das Original

### Pull Requests

Besonders willkommen:
- Neue Provider in `domains.csv`
- Übersetzungsverbesserungen
- Bug-Fixes
- Dokumentations-Updates

## 💬 Support

### Hilfe bekommen

1. **Dokumentation lesen** - Diese README und `man mw`
2. **Issues durchsuchen** - Vielleicht wurde dein Problem schon gelöst
3. **Neues Issue öffnen** - Mit detaillierter Fehlerbeschreibung

### Nützliche Ressourcen

- [Neomutt Dokumentation](https://neomutt.org/guide/)
- [mbsync Manual](https://isync.sourceforge.io/mbsync.html)
- [pass Manual](https://www.passwordstore.org/)
- [ArchWiki: Mutt](https://wiki.archlinux.org/title/Mutt)

## 📜 Lizenz

mutt-wizard ist freie/libre Software unter der **GPLv3 Lizenz**.

Siehe [LICENSE](LICENSE) Datei für Details.

## 🙏 Credits

- **[Luke Smith](https://github.com/LukeSmithxyz)** - Original mutt-wizard Entwickler
- **[Neomutt Team](https://neomutt.org/)** - Exzellenter E-Mail-Client
- **[isync/mbsync Entwickler](https://isync.sourceforge.io/)** - Zuverlässige IMAP-Sync
- **[pass Entwickler](https://www.passwordstore.org/)** - Sicheres Passwort-Management

---

**📧 Fragen oder Probleme?**
- [Codeberg Issues](https://codeberg.org/Sergius/mutt-wizzard-de/issues)
- [Original mutt-wizard](https://muttwizard.com/)

---

**⭐ Gefällt dir mutt-wizard?** - Star das Projekt auf [Codeberg](https://codeberg.org/Sergius/mutt-wizzard-de)!
