# Windows Privesc - Finding Sensitive Credentials in Windows

> **Focus:** Red Team Enumeration
> **Goal:** Identify exposed passwords, tokens, keys, and credential-related information that may be useful for further Windows privilege-escalation analysis.

---

## Table of Contents

* [1. Credential Hunting Mindset](#1-credential-hunting-mindset)
* [2. Search in Common File Types](#2-search-in-common-file-types)
* [3. Search in Scripts](#3-search-in-scripts)
* [4. Search in Configuration Files](#4-search-in-configuration-files)
* [5. Search in Backup Files](#5-search-in-backup-files)
* [6. Search in Windows Credential Manager](#6-search-in-windows-credential-manager)
* [7. Search in PuTTY](#7-search-in-putty)
* [8. Search in RDP Configuration Files](#8-search-in-rdp-configuration-files)
* [9. Search in PowerShell History](#9-search-in-powershell-history)
* [10. Search in Environment Variables](#10-search-in-environment-variables)
* [11. Search in Registry](#11-search-in-registry)
* [12. Search in Application Configuration](#12-search-in-application-configuration)
* [13. Search in Browser Data](#13-search-in-browser-data)
* [14. Search in SSH Configuration](#14-search-in-ssh-configuration)
* [15. Search in Database Configuration](#15-search-in-database-configuration)
* [16. Search in IIS / Web Configuration](#16-search-in-iis--web-configuration)
* [17. Search in Temporary and User Files](#17-search-in-temporary-and-user-files)
* [18. Credential Keywords](#18-credential-keywords)
* [19. Automated Credential Hunting](#19-automated-credential-hunting)
* [20. Quick Credential-Hunting Checklist](#20-quick-credential-hunting-checklist)

---

# 1. Credential Hunting Mindset

During Windows enumeration, credentials can be hidden in many different places.

Think:

```text
Windows Host
    |
    +-- Files
    +-- Scripts
    +-- Configuration
    +-- Applications
    +-- Credential Stores
    +-- Registry
    +-- User History
    +-- Remote Access Configuration
```

The objective is to find:

* Passwords
* Usernames
* API keys
* Tokens
* SSH keys
* Connection strings
* Service credentials
* Cloud credentials
* Remote-access credentials

---

# 2. Search in Common File Types

Start by identifying files that commonly contain credentials.

### Interesting Extensions

```text
.txt
.ini
.conf
.config
.xml
.json
.yml
.yaml
.ps1
.bat
.cmd
.vbs
.sql
.csv
.bak
.old
.backup
.rdp
.ppk
.pem
.key
```

### Common Locations

```text
C:\Users\
C:\ProgramData\
C:\Program Files\
C:\Program Files (x86)\
C:\Windows\Temp\
C:\Temp\
C:\Backup\
```

Also inspect application-specific directories.

---

# 3. Search in Scripts

Scripts are one of the most common places for hardcoded credentials.

### Interesting Script Types

```text
.ps1
.bat
.cmd
.vbs
.js
.py
```

### Search For

```text
password
passwd
pwd
username
user
credential
secret
token
apikey
api_key
connectionstring
```

### PowerShell

```powershell
Get-ChildItem C:\ -Recurse -Include *.ps1,*.bat,*.cmd -ErrorAction SilentlyContinue
```

Search contents:

```powershell
Select-String -Path "C:\path\*" `
    -Pattern "password|passwd|pwd|secret|token|apikey" `
    -ErrorAction SilentlyContinue
```

### What to Look For

```text
$username = "admin"
$password = "********"

$apiKey = "********"

connectionString="Server=...;User=...;Password=..."
```

---

# 4. Search in Configuration Files

Configuration files frequently contain application credentials.

### Important Extensions

```text
.ini
.conf
.config
.xml
.json
.yml
.yaml
```

### Common Examples

```text
web.config
app.config
settings.json
database.ini
config.xml
application.yml
```

### Search Keywords

```text
password
passwd
pwd
username
user
login
credential
secret
token
apikey
connectionString
connection_string
```

### Example

```text
[database]
username=admin
password=********
```

---

# 5. Search in Backup Files

Old configuration files can contain credentials that were removed from the current configuration.

Look for:

```text
*.bak
*.backup
*.old
*.orig
*.save
*.tmp
```

Example:

```text
web.config
web.config.bak

database.ini
database.ini.old
```

### Important Locations

```text
C:\Backup\
C:\Temp\
C:\Users\Public\
Application directories
Web application directories
```

---

# 6. Search in Windows Credential Manager

Windows Credential Manager can contain stored credentials.

### Enumeration

```cmd
cmdkey /list
```

Look for:

```text
Windows Credentials
Generic Credentials
Network targets
Remote resources
```

### Reference

[Microsoft — Credential Manager](https://learn.microsoft.com/en-us/windows-server/identity/windows-credential-manager)

---

# 7. Search in PuTTY

PuTTY is particularly interesting during Windows credential enumeration because users may store SSH connection information.

### Registry Location

```text
HKCU\Software\SimonTatham\PuTTY\Sessions
```

Enumerate:

```cmd
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions"
```

Inspect sessions:

```cmd
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions\<SessionName>"
```

Interesting information may include:

```text
HostName
PortNumber
UserName
PublicKeyFile
```

### Key Files

Look for:

```text
.ppk
.pem
.key
```

Typical locations:

```text
C:\Users\<user>\Documents\
C:\Users\<user>\Desktop\
C:\Users\<user>\Downloads\
```

> PuTTY session information can reveal usernames, hosts, and authentication-related configuration even when a plaintext password is not present.

---

# 8. Search in RDP Configuration Files

Remote Desktop configuration files can contain connection information.

Look for:

```text
*.rdp
```

Search:

```powershell
Get-ChildItem C:\Users -Recurse -Include *.rdp -ErrorAction SilentlyContinue
```

Interesting fields include:

```text
full address
username
gatewayhostname
remoteapplicationname
```

Also inspect:

```text
Desktop
Documents
Downloads
```

---

# 9. Search in PowerShell History

PowerShell history may reveal commands containing credentials or authentication information.

### History Location

```powershell
(Get-PSReadLineOption).HistorySavePath
```

Common location:

```text
C:\Users\<user>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\
```

Example:

```powershell
Get-Content (Get-PSReadLineOption).HistorySavePath
```

Search for:

```text
password
passwd
credential
token
secret
connect
login
```

---

# 10. Search in Environment Variables

Environment variables can contain application secrets.

### CMD

```cmd
set
```

### PowerShell

```powershell
Get-ChildItem Env:
```

Interesting names:

```text
PASSWORD
PASS
SECRET
TOKEN
API_KEY
APIKEY
USERNAME
DB_PASSWORD
```

---

# 11. Search in Registry

The Registry can contain application configuration and stored connection information.

### Common Areas

```text
HKCU
HKLM
HKU
```

Search application-specific locations.

Useful keyword concepts:

```text
password
username
credential
secret
token
connection
```

Example:

```cmd
reg query HKCU /f "password" /t REG_SZ /s
```

Another example:

```cmd
reg query HKLM /f "password" /t REG_SZ /s
```

> Registry searches can generate a lot of noise, so application-specific enumeration is usually more useful.

---

# 12. Search in Application Configuration

Installed applications may maintain their own credential stores and configuration files.

Inspect:

```text
C:\Program Files\
C:\Program Files (x86)\
C:\ProgramData\
%APPDATA%\
%LOCALAPPDATA%\
```

Look for directories related to:

```text
Backup software
Database software
FTP clients
SSH clients
VPN clients
Remote administration tools
Development tools
Cloud tools
Monitoring software
```

Search for:

```text
config
settings
credentials
password
secret
token
connection
```

---

# 13. Search in Browser Data

Browsers may contain sensitive authentication material.

Potential targets:

```text
Saved passwords
Cookies
Session information
Tokens
Browser profiles
```

Common application locations include:

```text
%LOCALAPPDATA%
%APPDATA%
```

Browsers of interest:

```text
Chrome / Chromium
Microsoft Edge
Firefox
```

The important enumeration question is:

```text
Browser
   ↓
User Profile
   ↓
Stored Authentication Data
```

---

# 14. Search in SSH Configuration

Look for SSH-related files.

### Common Files

```text
id_rsa
id_ed25519
id_ecdsa
authorized_keys
known_hosts
config
```

### Common Location

```text
C:\Users\<username>\.ssh\
```

Search:

```powershell
Get-ChildItem C:\Users -Recurse -Force -ErrorAction SilentlyContinue |
Where-Object {$_.FullName -match "\\\.ssh\\"}
```

Look for:

```text
Private Keys
SSH Configurations
Usernames
Known Hosts
```

---

# 15. Search in Database Configuration

Database applications frequently store connection strings.

Look for:

```text
connectionString
connection_string
server
database
uid
user
username
pwd
password
```

Common database configuration files:

```text
web.config
app.config
*.ini
*.xml
*.json
*.yml
```

Example:

```text
Server=db01;
Database=production;
User=appuser;
Password=********;
```

---

# 16. Search in IIS / Web Configuration

For Windows web servers, inspect IIS and application configuration.

Important files include:

```text
web.config
applicationHost.config
```

Common locations:

```text
C:\inetpub\
C:\Windows\System32\inetsrv\config\
```

Search for:

```text
connectionString
password
username
user
identity
credential
```

Example:

```powershell
Select-String `
    -Path "C:\inetpub\**\web.config" `
    -Pattern "password|connectionString|username" `
    -ErrorAction SilentlyContinue
```

---

# 17. Search in Temporary and User Files

Users frequently leave sensitive information in common working directories.

Check:

```text
Desktop
Documents
Downloads
Temp
Public
Pictures
Scripts
Backup
```

Interesting files:

```text
password.txt
credentials.txt
config.bak
backup.zip
database.sql
notes.txt
```

Search filenames:

```powershell
Get-ChildItem C:\Users -Recurse -Force `
    -ErrorAction SilentlyContinue |
Where-Object {
    $_.Name -match "pass|cred|secret|config|backup|token"
}
```

---

# 18. Credential Keywords

A good credential-hunting keyword list:

### Authentication

```text
password
passwd
pwd
credential
credentials
auth
authentication
login
```

### User Information

```text
username
user
userid
user_id
login
account
```

### Secrets

```text
secret
token
access_token
refresh_token
apikey
api_key
private_key
```

### Database

```text
connectionString
connection_string
db_password
database_password
db_user
database_user
```

### Cloud

```text
access_key
secret_key
aws_access_key
aws_secret
azure
client_secret
```

---

# 19. Automated Credential Hunting

Manual enumeration is useful, but automated tools can quickly identify common credential locations.

## WinPEAS

[PEASS-ng / WinPEAS](https://github.com/peass-ng/PEASS-ng)

Useful for broad Windows enumeration and identifying potentially interesting credential-related artifacts.

---

## Seatbelt

[GhostPack / Seatbelt](https://github.com/GhostPack/Seatbelt)

Performs Windows security enumeration and can inspect several credential-related areas.

---

## LaZagne

[LaZagne](https://github.com/AlessandroZ/LaZagne)

Designed to recover credentials stored by various applications.

Use only in authorized environments.

---

# 20. Quick Credential-Hunting Checklist

```text
[ ] Current user and groups
[ ] Credential Manager
[ ] PowerShell history
[ ] PowerShell scripts
[ ] BAT/CMD scripts
[ ] INI files
[ ] CONF files
[ ] CONFIG files
[ ] XML files
[ ] JSON files
[ ] YAML files
[ ] Backup files
[ ] Temporary files
[ ] PuTTY sessions
[ ] PPK / PEM / KEY files
[ ] RDP files
[ ] SSH directory
[ ] Environment variables
[ ] Registry
[ ] Application configuration
[ ] Database configuration
[ ] IIS web.config
[ ] Browser profiles
[ ] Desktop / Documents / Downloads
[ ] ProgramData
[ ] Service/application configuration
```

---

# Credential Enumeration Mental Model

```text
                 WINDOWS HOST
                      |
      +---------------+---------------+
      |               |               |
     FILES        APPLICATIONS     WINDOWS
      |               |            STORES
      |               |               |
   Scripts         PuTTY         Credential Manager
   INI/CONF        Browsers      Registry
   XML/JSON        Databases     Environment
   Backup          SSH           PowerShell History
      |               |               |
      +---------------+---------------+
                      |
                      v
              SENSITIVE CREDENTIAL
                      |
                      v
             Identify Owner / Context
                      |
                      v
             Further Privilege
             Escalation Analysis
```

## Core Principle

> **For Windows credential hunting, enumerate broadly first: scripts → configuration files → application data → remote-access files → credential stores → registry → user history → backup/temporary files.**

The key is to **find where credentials are exposed**, not to jump directly into credential dumping or exploitation.
