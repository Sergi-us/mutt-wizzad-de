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
- ✅ **PGP/GPG** - E-Mail-Verschlüsselung und -Signierung
- ✅ **mailsync** - Automatische Synchronisation mit Benachrichtigungen
- ✅ **Mehrere Konten** - Bis zu 9 E-Mail-Konten gleichzeitig
- ✅ **TLSType** - umgestellt von SSLType

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

| Befehl                           | Beschreibung                               |
|----------------------------------|--------------------------------------------|
| `mw -a email@example.com`        | E-Mail-Konto hinzufügen                    |
| `mw -l`                          | Alle konfigurierten Konten auflisten       |
| `mw -d`                          | Konto löschen (interaktiv)                 |
| `mw -D email@example.com`        | Konto ohne Bestätigung löschen             |
| `mw -g email@example.com`        | PGP-Verschlüsselung für Konto aktivieren   |
| `mw -t 30`                       | Auto-Sync alle 30 Minuten aktivieren       |
| `mw -T`                          | Auto-Sync mit Standard (10 Min) aktivieren |
| `mw -r`                          | Konto-Nummern neu ordnen                   |
| `mailsync`                       | Alle Konten synchronisieren                |
| `mailsync email@example.com`     | Bestimmtes Konto synchronisieren           |
| `pass edit mw-email@example.com` | Passwort ändern                            |

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

| Taste                                                                     | Funktion                                            |
|---------------------------------------------------------------------------|-----------------------------------------------------|
| <kbd>m</kbd>                                                              | Neue E-Mail schreiben                               |
| <kbd>j</kbd>/<kbd>k</kbd>                                                 | Hoch/Runter navigieren                              |
| <kbd>d</kbd>/<kbd>u</kbd>                                                 | Seite runter/hoch                                   |
| <kbd>l</kbd>                                                              | E-Mail/Anhang öffnen                                |
| <kbd>h</kbd>                                                              | Zurück                                              |
| <kbd>r</kbd>/<kbd>R</kbd>                                                 | Antworten/Allen antworten                           |
| <kbd>s</kbd>                                                              | E-Mail/Anhang speichern                             |
| <kbd>gs</kbd>, <kbd>gi</kbd>, <kbd>ga</kbd>, <kbd>gd</kbd>, <kbd>gS</kbd> | Zu Sent/Inbox/Archive/Drafts/Spam                   |
| <kbd>M</kbd>, <kbd>C</kbd>                                                | Verschieben/Kopieren (+ Mailbox-Buchstabe)          |
| <kbd>i1</kbd>-<kbd>i9</kbd>                                               | Zu Konto 1-9 wechseln                               |
| <kbd>a</kbd>                                                              | Adresse zu abook hinzufügen                         |
| <kbd>Tab</kbd>                                                            | Adresse aus abook vervollständigen                  |
| <kbd>?</kbd>                                                              | Alle Tastenkombinationen anzeigen                   |
| <kbd>Ctrl+j</kbd>/<kbd>Ctrl+k</kbd>                                       | In Sidebar hoch/runter                              |
| <kbd>Ctrl+o</kbd>                                                         | Mailbox aus Sidebar öffnen                          |
| <kbd>Ctrl+b</kbd>                                                         | URL-Menü öffnen                                     |
| <kbd>Ctrl+f</kbd>                                                         | Notmuch-Suche                                       |
| <kbd>p</kbd>                                                              | E-Mail verschlüsseln/signieren (in Compose-Ansicht) |
| <kbd>K</kbd>                                                              | Öffentlichen Schlüssel holen (in Compose-Ansicht)   |

## 🔐 PGP-Verschlüsselung einrichten

mutt-wizard unterstützt vollständige PGP/GPG-Verschlüsselung und -Signierung von E-Mails.

### Voraussetzungen

1. **GPG-Schlüssel für E-Mail-Verschlüsselung erstellen**

**Best Practice:** Separater Schlüssel für E-Mail (nicht den pass-Schlüssel verwenden!)

```bash
# Schlüssel generieren
gpg --full-generate-key

# Wähle:
# - RSA (sign and encrypt)
# - 4096 Bit
# - Ihr Name ("Vorname Nachname"): Unwichtig/Unnötig
# - Email-Adresse: hier die Mailadresse eintragen (wichtig damit der Schlüssel automatisch gefunden wird)
# - Gültigkeitsdauer und andere angaben nach belieben eingeben oder weglassen
```

2. **Fingerprint notieren (unnötig, sollte automatisch finden) **

```bash
# Liste alle Schlüssel mit Fingerprints
gpg --list-secret-keys --keyid-format=long

# Beispiel-Ausgabe:
# sec   rsa4096/1234ABCD5678EFGH 2025-01-15 [SC]
#       0123456789ABCDEF0123456789ABCDEF01234567  ← Das ist der Fingerprint
# uid   [ultimativ] Dein Name <deine@email.com>
```

### PGP für Account aktivieren

```bash
# PGP-Verschlüsselung einrichten
mw -g deine@email.com

# Das Skript wird:
# 1. Automatisch nach einem passenden Schlüssel suchen
# 2. Schlüssel-Details anzeigen
# 3. Nach Bestätigung fragen
# 4. Oder: Manuellen Fingerprint abfragen
```

**Interaktiver Ablauf:**
```
=== PGP-Verschlüsselung einrichten für: deine@email.com ===

🔍 Automatisch gefundener Schlüssel:

=== Gefundener PGP-Schlüssel ===
sec   rsa4096/1234ABCD5678EFGH 2025-01-15 [SC]
uid   [ultimativ] Dein Name <deine@email.com>
=================================

Diesen Schlüssel verwenden? [Y/n]
```

### Aktivierte Einstellungen

Nach `mw -g` sind folgende Einstellungen aktiv:

- ✅ **Automatisches Signieren**: Alle ausgehenden E-Mails werden signiert
- ⚙️ **Manuelle Verschlüsselung**: Per Tastendruck `p` beim Verfassen

**Optionale Einstellungen** (auskommentiert in `~/.config/mutt/accounts/EMAIL.muttrc`):
```muttrc
# Automatisch verschlüsseln wenn Empfänger-Schlüssel vorhanden
#set crypt_opportunistic_encrypt = yes

# Kopien an dich selbst auch verschlüsseln
#set pgp_self_encrypt = yes
```

### E-Mails verschlüsseln und signieren

#### Beim Verfassen einer E-Mail:

1. **Neue E-Mail starten**: Taste `m` (zum testen an `bot@autocrypt.org` oder `adele@gnupp.de` )
2. **Nach dem Schreiben**: In der Compose-Ansicht
3. **PGP-Optionen ändern**: Taste `p`

**PGP-Menü:**
```
PGP (s):  Sign
PGP (e):  Encrypt
PGP (b):  Sign & Encrypt
PGP (f):  Forget passphrase
PGP (c):  Clear
```

- `s` - Nur signieren
- `e` - Nur verschlüsseln  
- `b` - Beides (empfohlen)
- `c` - Nichts (PGP deaktivieren)

4. **Senden**: Taste `y`

### Verschlüsselte E-Mails empfangen

**Automatisch:** Neomutt entschlüsselt automatisch beim Öffnen der E-Mail.

```
# GPG-Passphrase wird abgefragt
# Nach Eingabe wird E-Mail entschlüsselt angezeigt
```

**Signatur-Überprüfung:**
- ✅ Grüner Text: `[-- Good signature from "Name <email@example.com>" --]`
- ❌ Roter Text: `[-- BAD signature from "..." --]`

### Öffentliche Schlüssel verwalten

#### Automatische Schlüsselsuche

mutt-wizard ist so konfiguriert, dass Neomutt **versucht** nach fehlenden Schlüsseln zu suchen:

**Funktioniert automatisch wenn:**
- ✅ Schlüssel auf `keys.openpgp.org` hochgeladen wurde
- ✅ Mailprovider WKS (Web Key Service) unterstützt (z.B. Posteo, Mailbox.org)

**Funktioniert NICHT automatisch wenn:**
- ❌ Schlüssel nur per E-Mail oder Website verteilt wurde
- ❌ Schlüssel auf privatem Keyserver liegt
- ❌ Empfänger keinen öffentlichen Schlüssel veröffentlicht hat

**Keyserver-Hierarchie:**
1. WKS (Web Key Service) vom Mailprovider
2. `keys.openpgp.org` (öffentlicher Keyserver)
3. Lokaler Keyring

**Manuelle Schlüsselsuche in Neomutt:**

In der Compose-Ansicht (beim E-Mail schreiben):
- Taste `K` drücken
- E-Mail-Adresse eingeben
- Neomutt versucht Schlüssel zu holen

**Wichtig:** Wenn die automatische Suche fehlschlägt, musst du den Schlüssel manuell importieren (siehe unten).

#### Schlüssel manuell importieren

**Aus einer E-Mail:**
```bash
# 1. Anhang speichern (Taste 's' in Neomutt)
# 2. Importieren
gpg --import schluessel.asc
```

**Von einem Keyserver (Kommandozeile):**
```bash
# Von keys.openpgp.org
gpg --keyserver keys.openpgp.org --recv-keys 1234ABCD5678EFGH

# Nach Email-Adresse suchen
gpg --keyserver keys.openpgp.org --search-keys email@example.com
```

**Via WKS (Web Key Service):**
```bash
# Versuche automatisch von Mailserver zu holen (z.B. Posteo, Mailbox.org)
gpg --locate-keys email@example.com
```

#### Schlüssel-Fingerprint verifizieren

**WICHTIG:** Immer Fingerprints über einen zweiten Kanal verifizieren!

```bash
# Fingerprint anzeigen
gpg --fingerprint email@example.com

# Per Telefon, Signal, persönlich, etc. bestätigen
```

#### Schlüssel signieren (Trust setzen)

```bash
# Schlüssel signieren = bestätigen, dass er echt ist
gpg --sign-key email@example.com

# Trust-Level setzen
gpg --edit-key email@example.com
gpg> trust
# Wähle: 4 (volle Vertrauen) oder 5 (ultimativ)
gpg> save
```

### Eigenen öffentlichen Schlüssel veröffentlichen

#### Export als Datei

```bash
# Öffentlichen Schlüssel exportieren
gpg --armor --export deine@email.com > mein-schluessel.asc

# Per E-Mail versenden oder auf Website hochladen
```

#### Auf Keyserver hochladen

```bash
# Zum öffentlichen Keyserver
gpg --send-keys DEIN_FINGERPRINT

# Zu bestimmtem Server
gpg --keyserver keys.openpgp.org --send-keys DEIN_FINGERPRINT
```

#### Via WKS (empfohlen bei Posteo, Mailbox.org, etc.)

In Neomutt:
1. Index-Ansicht öffnen
2. Taste `Ctrl+g` drücken
3. Anweisungen folgen

Oder manuell:
```bash
gpg --locate-keys deine@email.com
# Falls WKS unterstützt wird, veröffentlicht dein Provider den Schlüssel
```

### Testen der Verschlüsselung

#### Einfacher Test-Workflow: An dich selbst

**Der einfachste Test ist eine verschlüsselte E-Mail an dich selbst:**

```bash
# 1. Neomutt öffnen
neomutt

# 2. Neue Mail an deine eigene Adresse
m
An: deine@email.com
Betreff: PGP-Test
Nachricht: Dies ist ein Test der Verschlüsselung

# 3. In Compose-Ansicht:
# Taste 'p' → Wähle 'b' (Sign & Encrypt)

# 4. Senden
# Taste 'y'

# 5. Mail empfangen und lesen
mailsync
neomutt
# → Die Mail wird automatisch entschlüsselt und signiert angezeigt
```

**Vorteile:**
- ✅ Dein eigener Schlüssel ist garantiert verfügbar
- ✅ Testet Signierung UND Verschlüsselung
- ✅ Testet Entschlüsselung beim Lesen
- ✅ Keine externen Abhängigkeiten

#### Workflow mit einem Kontakt

**Wenn du an jemand anderen verschlüsseln möchtest:**

**1. Schlüssel besorgen:**

Frage deinen Kontakt nach seinem öffentlichen Schlüssel via:
- E-Mail-Anhang
- Keyserver-Link
- Fingerprint zum Verifizieren

**2. Schlüssel importieren:**
```bash
# Von Keyserver (wenn Fingerprint bekannt)
gpg --recv-keys FINGERPRINT

# Oder in Neomutt: Taste K in Compose-Ansicht
# Email eingeben: kontakt@example.com
```

**3. Verschlüsselt mailen:**
```bash
# Normale Mail schreiben
# In Compose: p → b (Sign & Encrypt)
# Senden: y
```

#### Test mit öffentlichen Test-Servern

**FlowCrypt Test-Service** (✅ empfohlen):

FlowCrypt bietet einen automatischen Test-Service für PGP-Verschlüsselung.

```bash
# 1. Schlüssel automatisch holen
gpg --locate-keys human@flowcrypt.com

# Fingerprint verifizieren (optional):
# 6BF1 6EE1 ECE7 A66C 4B66  36DF 0C9C 2E6A 4D27 3C6F

# 2. In Neomutt verschlüsselte Mail senden
neomutt
m
An: human@flowcrypt.com
Betreff: PGP Test
Nachricht: Testing encryption with FlowCrypt

# Compose: p → b (Sign & Encrypt)
# Senden: y

# 3. Antwort kommt automatisch verschlüsselt zurück
mailsync
neomutt
```

**Was du sehen solltest:**
- ✅ Mail wird erfolgreich verschlüsselt und gesendet
- ✅ Automatische Antwort kommt verschlüsselt zurück
- ✅ Neomutt entschlüsselt automatisch beim Öffnen
- ✅ Grüne "Good signature" Meldung

**Autocrypt-Bot**:
```bash
# An: bot@autocrypt.org
# Erste Mail: NUR SIGNIEREN (p → s)
# Der Bot antwortet mit seinem Schlüssel
# Zweite Mail: Verschlüsseln möglich
```

**⚠️ Hinweis:** Edward/Adele-Bots haben oft abgelaufene Schlüssel.

### Troubleshooting

#### "No valid OpenPGP data found"

**Problem:** Empfänger-Schlüssel fehlt

**Lösung:**
```bash
# Schlüssel vom Empfänger holen
gpg --locate-keys empfaenger@example.com
# oder
gpg --recv-keys EMPFAENGER_KEY_ID
```

#### "Decryption failed: No secret key"

**Problem:** Dein eigener privater Schlüssel fehlt oder ist nicht entsperrt

**Lösung:**
```bash
# Prüfe ob Schlüssel vorhanden
gpg --list-secret-keys

# GPG-Agent neustarten
gpgconf --kill gpg-agent
```

#### "Unusable public key"

**Problem:** Schlüssel abgelaufen oder nicht vertrauenswürdig

**Lösung:**
```bash
# Trust-Level setzen (siehe oben)
# Oder neuen Schlüssel holen
gpg --refresh-keys
```

#### Passphrase-Caching

**Problem:** GPG fragt ständig nach Passphrase

**Lösung 1 - pam-gnupg** (empfohlen):
```bash
yay -S pam-gnupg
# Entsperrt GPG automatisch beim Login
# Anleitung: https://github.com/cruegge/pam-gnupg
```

**Lösung 2 - Cache-Zeit erhöhen:**
```bash
# ~/.gnupg/gpg-agent.conf
default-cache-ttl 3600        # 1 Stunde
max-cache-ttl 86400          # 24 Stunden

# GPG-Agent neustarten
gpgconf --kill gpg-agent
```

### Best Practices

1. ✅ **Separater Schlüssel**: Unterschiedliche Schlüssel für pass und E-Mail
2. ✅ **Backup erstellen**: Privaten Schlüssel sicher aufbewahren
   ```bash
   gpg --armor --export-secret-keys FINGERPRINT > backup-private.asc
   # Auf USB-Stick, verschlüsselte Cloud, etc.
   ```
3. ✅ **Revocation Certificate**: Sofort nach Erstellung
   ```bash
   gpg --gen-revoke FINGERPRINT > revoke.asc
   # Sicher aufbewahren für Notfälle
   ```
4. ✅ **Fingerprint verifizieren**: Immer über zweiten Kanal
5. ✅ **Regelmäßig erneuern**: Ablaufdatum setzen (z.B. 2 Jahre)
6. ✅ **Subkeys verwenden**: Für erweiterte Sicherheit

### Weiterführende Ressourcen

- [GPG Handbuch (Deutsch)](https://gnupg.org/documentation/manuals/gnupg/)
- [Email Self-Defense (FSF)](https://emailselfdefense.fsf.org/)
- [Neomutt PGP Guide](https://neomutt.org/guide/security.html)
- [OpenPGP Best Practices](https://riseup.net/en/security/message-security/openpgp/best-practices)

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
