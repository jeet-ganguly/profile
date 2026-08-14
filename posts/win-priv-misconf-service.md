# Windows Privilege Escalation via Misconfigured Services

> **Focus:** Red Team Enumeration → Identify Misconfigured Windows Services → Determine Potential Privilege Escalation Path
> **Level:** OSCP / Red Team Practice
> **Scope:** Enumeration techniques and understanding of service-based attack vectors.

---

## Table of Contents

* [1. Overview](#1-overview)
* [2. Service Privilege Escalation Mindset](#2-service-privilege-escalation-mindset)
* [3. Enumerate Windows Services](#3-enumerate-windows-services)
* [4. Identify Service Accounts](#4-identify-service-accounts)
* [5. Inspect Service Configuration](#5-inspect-service-configuration)
* [6. Check Service Binary Paths](#6-check-service-binary-paths)
* [7. Check Service Binary Permissions](#7-check-service-binary-permissions)
* [8. Check Service Directory Permissions](#8-check-service-directory-permissions)
* [9. Unquoted Service Paths](#9-unquoted-service-paths)
* [10. Weak Service Permissions](#10-weak-service-permissions)
* [11. Weak Binary Permissions](#11-weak-binary-permissions)
* [12. Weak Service Directory Permissions](#12-weak-service-directory-permissions)
* [13. DLL Search Order Hijacking](#13-dll-search-order-hijacking)
* [14. Service Configuration and Registry](#14-service-configuration-and-registry)
* [15. Service Recovery Actions](#15-service-recovery-actions)
* [16. Environment Variables in Service Paths](#16-environment-variables-in-service-paths)
* [17. Interesting Service Properties](#17-interesting-service-properties)
* [18. Manual Enumeration Workflow](#18-manual-enumeration-workflow)
* [19. Automated Enumeration Tools](#19-automated-enumeration-tools)
* [20. OSCP Enumeration Checklist](#20-oscp-enumeration-checklist)
* [21. Attack-Path Decision Tree](#21-attack-path-decision-tree)
* [22. References](#22-references)

---

# 1. Overview

Windows Services are background processes managed by the **Service Control Manager (SCM)**.

A service normally has:

```text
Service
   |
   +-- Name
   +-- Binary
   +-- Start Type
   +-- Service Account
   +-- Configuration
   +-- Permissions
```

Services become interesting for privilege escalation when a low-privileged user can influence something that is executed by a **higher-privileged service account**.

Typical attack surface:

```text
Low-Privileged User
        |
        v
Service Enumeration
        |
        v
Find Misconfiguration
        |
   +----+----+
   |    |    |
   v    v    v
Binary Path  Service ACL  DLL/Directory
   |    |        |           |
   +----+--------+-----------+
             |
             v
       Privileged Service
```

---

# 2. Service Privilege Escalation Mindset

For every interesting service, answer:

```text
1. What is the service?
2. Which account runs it?
3. What executable does it launch?
4. Where is that executable located?
5. Who can modify the executable?
6. Who can modify its directory?
7. Who can modify the service configuration?
8. Is the executable path quoted?
9. Does it load DLLs?
10. Does the service have recovery actions?
```

The most important relationship is:

```text
Service Privilege
        +
Attacker Control
        =
Potential Privilege Escalation
```

---

# 3. Enumerate Windows Services

## CMD

```cmd
sc query
```

More detailed enumeration:

```cmd
sc query state= all
```

---

## PowerShell

```powershell
Get-Service
```

For more useful information:

```powershell
Get-CimInstance Win32_Service |
Select-Object Name, DisplayName, State, StartMode, StartName, PathName
```

This is one of the most useful commands for service enumeration.

Example output:

```text
Name        : BackupService
State       : Running
StartMode   : Auto
StartName   : LocalSystem
PathName    : C:\Program Files\Backup\backup.exe
```

---

# 4. Identify Service Accounts

The service account determines the potential privilege level.

Important accounts:

```text
LocalSystem
LocalService
NetworkService
Administrator
Domain User
Domain Service Account
```

### PowerShell

```powershell
Get-CimInstance Win32_Service |
Select Name,StartName
```

### Interesting Services

Prioritize services running as:

```text
NT AUTHORITY\SYSTEM
Administrator
Local Administrator
Privileged Domain Account
```

Conceptually:

```text
Service
   |
   +-- Runs as SYSTEM
          |
          +-- Can attacker influence execution?
                    |
                    +-- Yes → High Priority
```

---

# 5. Inspect Service Configuration

Use:

```cmd
sc qc <service>
```

Example:

```cmd
sc qc MyService
```

Important fields:

```text
SERVICE_NAME
START_TYPE
BINARY_PATH_NAME
SERVICE_START_NAME
```

PowerShell:

```powershell
Get-CimInstance Win32_Service -Filter "Name='MyService'" |
Format-List *
```

### What to Record

```text
Service Name
Binary Path
Start Account
Startup Type
Service State
Dependencies
```

---

# 6. Check Service Binary Paths

The **binary path** tells you what executable the service starts.

Example:

```text
C:\Program Files\Example\service.exe
```

Enumeration:

```powershell
Get-CimInstance Win32_Service |
Select Name,PathName,StartName
```

Ask:

```text
Where is the executable?
        ↓
Can my current user modify it?
        ↓
Can my current user modify its directory?
```

---

# 7. Check Service Binary Permissions

Finding a service binary is not enough.

You need to determine whether your current user can modify it.

Use:

```cmd
icacls "C:\Path\service.exe"
```

Example:

```cmd
icacls "C:\Program Files\Example\service.exe"
```

Look for permissions such as:

```text
F   Full Control
M   Modify
W   Write
```

### Interesting Situation

```text
Privileged Service
      |
      v
service.exe
      |
      v
Current User = Modify/Write
      |
      v
Potential Service-Based Privesc
```

---

# 8. Check Service Directory Permissions

Sometimes the executable itself is protected, but the directory containing it is writable.

Example:

```text
C:\Program Files\Example\service.exe
```

Check:

```cmd
icacls "C:\Program Files\Example"
```

Also inspect parent directories:

```cmd
icacls "C:\Program Files"
```

### Attack Surface

```text
Service
  |
  v
C:\Program Files\Example\service.exe
  |
  +-- service.exe protected
  |
  +-- Example directory writable
             |
             v
       Potential Abuse
```

This is why checking **both the binary and its parent directories** is important.

---

# 9. Unquoted Service Paths

An **unquoted service path** occurs when a service executable path contains spaces but is not enclosed in quotation marks.

Example:

```text
C:\Program Files\Example App\service.exe
```

Instead of:

```text
"C:\Program Files\Example App\service.exe"
```

Windows may interpret the path in multiple ways during process creation.

### Enumeration

Find service paths:

```powershell
Get-CimInstance Win32_Service |
Select Name,PathName
```

Look for:

```text
C:\Program Files\
C:\Program Files (x86)\
C:\Some Directory\
```

without surrounding quotes.

### Example

```text
C:\Program Files\Example App\service.exe
```

Potential path interpretation:

```text
C:\Program.exe
C:\Program Files\Example.exe
C:\Program Files\Example App\service.exe
```

The important question is:

> **Can the current user write to one of the directories where Windows may search for the executable?**

---

# 10. Weak Service Permissions

A service has its own security descriptor and access permissions.

A low-privileged user may sometimes have excessive permissions over a service.

### Inspect Service Security

```cmd
sc sdshow <service>
```

Example:

```cmd
sc sdshow MyService
```

The output is a **Security Descriptor Definition Language (SDDL)** string.

For OSCP-level enumeration, understand the concept:

```text
Service
   |
   v
Service ACL
   |
   v
Can current user modify service?
   |
   +-- Yes → Interesting
```

Potentially interesting permissions include the ability to:

* Change service configuration
* Start/stop the service
* Modify service-related settings

---

# 11. Weak Binary Permissions

A service may run as SYSTEM while its executable is writable by a low-privileged user.

Example:

```text
Service Account:
NT AUTHORITY\SYSTEM

Binary:
C:\Tools\backup.exe

Permissions:
Users → Modify
```

This is a high-value finding.

Enumeration:

```cmd
icacls "C:\Tools\backup.exe"
```

### Attack Model

```text
SYSTEM Service
      |
      v
Privileged Binary
      |
      v
Weak File ACL
      |
      v
Low-Privileged User Can Modify
      |
      v
Potential Privilege Escalation
```

---

# 12. Weak Service Directory Permissions

Check the entire path.

Example:

```text
C:\Program Files\Backup Service\backup.exe
```

Check:

```cmd
icacls "C:\Program Files\Backup Service"
```

If the directory is writable:

```text
Writable Directory
       |
       v
Privileged Service Binary
       |
       v
Service Execution
```

### Important

Do not only check the final file.

Check:

```text
Executable
   ↓
Immediate Directory
   ↓
Parent Directory
   ↓
Relevant Path Components
```

---

# 13. DLL Search Order Hijacking

Some Windows services load DLLs dynamically.

If a service searches writable directories for required DLLs, a low-privileged user may potentially influence which DLL is loaded.

### Enumeration Questions

```text
Which DLLs does the executable load?
        ↓
Where are those DLLs expected?
        ↓
Are any search locations writable?
        ↓
Does the service run with high privileges?
```

### Useful Tools

Process analysis tools such as:

* Process Monitor
* Process Explorer
* `dumpbin`
* PE analysis tools

can help identify DLL dependencies.

### Attack Model

```text
Privileged Service
       |
       v
Loads DLL
       |
       v
Searches Locations
       |
       v
Writable Search Location
       |
       v
Potential DLL Hijacking
```

---

# 14. Service Configuration and Registry

Windows service configuration is represented in the Registry.

Important location:

```text
HKLM\SYSTEM\CurrentControlSet\Services\
```

Each service normally has its own subkey:

```text
HKLM\SYSTEM\CurrentControlSet\Services\<ServiceName>
```

Enumeration:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Services\<ServiceName>"
```

Important values include:

```text
ImagePath
ObjectName
Start
Type
DisplayName
DependOnService
```

### Important Relationship

```text
Service
   |
   +-- Registry Configuration
   |
   +-- ImagePath
   |
   +-- Service Account
```

Registry permissions should also be considered when assessing service misconfigurations.

---

# 15. Service Recovery Actions

Windows Services can have recovery actions.

These may specify actions such as:

```text
Restart Service
Run Program
Reboot Computer
```

Inspect service configuration:

```cmd
sc qfailure <service>
```

Example:

```cmd
sc qfailure MyService
```

### Enumeration Questions

```text
Does the service have recovery actions?
        ↓
Does recovery execute another program?
        ↓
What account executes that action?
        ↓
Can the referenced program be modified?
```

---

# 16. Environment Variables in Service Paths

Service paths may contain environment variables.

Example:

```text
%ProgramFiles%\Example\service.exe
```

or:

```text
%SystemRoot%\System32\service.exe
```

During enumeration, resolve the variables and determine the actual executable location.

Useful command:

```cmd
echo %ProgramFiles%
echo %SystemRoot%
```

PowerShell:

```powershell
$env:ProgramFiles
$env:SystemRoot
```

Then inspect the resolved path:

```cmd
icacls "C:\Program Files\Example\service.exe"
```

---

# 17. Interesting Service Properties

For every service, build a small table:

| Property         | Question                               |
| ---------------- | -------------------------------------- |
| Service Name     | What service is it?                    |
| State            | Is it running?                         |
| Start Mode       | Automatic/manual?                      |
| Start Account    | Who runs it?                           |
| Binary Path      | What executable runs?                  |
| Binary ACL       | Can I modify it?                       |
| Directory ACL    | Can I modify its directory?            |
| Service ACL      | Can I modify service configuration?    |
| Path Quoting     | Is the path quoted?                    |
| DLL Dependencies | Does it load DLLs?                     |
| Recovery         | Does it execute recovery actions?      |
| Registry         | Can service configuration be modified? |

---

# 18. Manual Enumeration Workflow

A practical OSCP workflow:

```text
               Enumerate Services
                       |
                       v
              Identify Interesting
                  Service Accounts
                       |
                       v
               Inspect Binary Path
                       |
          +------------+------------+
          |                         |
          v                         v
      Quoted?                  Unquoted?
          |                         |
          v                         v
     Check ACLs              Check Path ACLs
          |                         |
          +------------+------------+
                       |
                       v
               Check Binary ACL
                       |
                       v
              Check Directory ACL
                       |
                       v
              Check Service ACL
                       |
                       v
             Check Registry Config
                       |
                       v
              Check DLL Dependencies
                       |
                       v
             Check Recovery Actions
```

---

# 19. Automated Enumeration Tools

## WinPEAS

[PEASS-ng / WinPEAS](https://github.com/peass-ng/PEASS-ng)

Useful for identifying:

* Interesting services
* Unquoted service paths
* Weak permissions
* Writable service binaries
* Other Windows privilege-escalation conditions

---

## PowerUp

[PowerSploit / PowerUp](https://github.com/PowerShellMafia/PowerSploit)

Designed to identify common Windows privilege-escalation misconfigurations.

Service-related checks include concepts such as:

```text
Service permissions
Unquoted paths
Writable service binaries
Service configuration
```

---

## SharpUp

[GhostPack / SharpUp](https://github.com/GhostPack/SharpUp)

A C# implementation focused on Windows privilege-escalation enumeration.

---

## AccessChk

[Microsoft Sysinternals — AccessChk](https://learn.microsoft.com/en-us/sysinternals/downloads/accesschk)

Useful for examining effective permissions on:

```text
Files
Directories
Services
Registry keys
```

---

# 20. OSCP Enumeration Checklist

When you encounter a Windows host, check:

### Service Discovery

```text
[ ] List all services
[ ] Identify running services
[ ] Identify automatic services
[ ] Identify unusual/custom services
```

### Service Account

```text
[ ] Identify StartName
[ ] Look for SYSTEM
[ ] Look for Administrator
[ ] Look for privileged domain accounts
```

### Binary

```text
[ ] Identify ImagePath
[ ] Check if path is quoted
[ ] Locate executable
[ ] Check executable permissions
[ ] Check parent directory permissions
```

### Service ACL

```text
[ ] Inspect service security descriptor
[ ] Determine whether current user can modify service configuration
```

### Additional Attack Surface

```text
[ ] Check DLL dependencies
[ ] Check DLL search locations
[ ] Check service registry configuration
[ ] Check recovery actions
[ ] Check service dependencies
```

---

# 21. Attack-Path Decision Tree

```text
                  Service Found
                       |
                       v
              Runs as Privileged User?
                  /          \
                No            Yes
                |              |
             Lower          Continue
             Priority          |
                               v
                       What does it run?
                               |
                               v
                         Binary Path
                               |
               +---------------+---------------+
               |               |               |
               v               v               v
          Writable?       Unquoted?        DLL Loading?
               |               |               |
              Yes              Yes             Yes
               |               |               |
               v               v               v
          High Priority    Check Path       Check DLL
               |               ACLs          Search Paths
               |               |               |
               +---------------+---------------+
                               |
                               v
                      Check Service ACL
                               |
                               v
                      Check Registry ACL
                               |
                               v
                    Determine Attack Path
```

---

# 22. References

* [Microsoft — Windows Services](https://learn.microsoft.com/en-us/windows/win32/services/services)
* [Microsoft — Service Control Manager](https://learn.microsoft.com/en-us/windows/win32/services/service-control-manager)
* [Microsoft — `sc.exe`](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/sc-query)
* [Microsoft — Task Scheduler](https://learn.microsoft.com/en-us/windows/win32/taskschd/task-scheduler-2-0)
* [Microsoft Sysinternals — AccessChk](https://learn.microsoft.com/en-us/sysinternals/downloads/accesschk)
* [PEASS-ng / WinPEAS](https://github.com/peass-ng/PEASS-ng)
* [PowerSploit / PowerUp](https://github.com/PowerShellMafia/PowerSploit)
* [GhostPack / SharpUp](https://github.com/GhostPack/SharpUp)
* [HackTricks — Windows Privilege Escalation](https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html)

---

# Quick Mental Model

```text
                WINDOWS SERVICES
                       |
                       v
                Enumerate Services
                       |
                       v
              Find Privileged Service
                       |
                       v
             Identify Binary / Path
                       |
        +--------------+--------------+
        |              |              |
        v              v              v
   Weak Binary    Unquoted Path   Weak Service ACL
     ACL              |              |
        |              |              |
        +--------------+--------------+
                       |
                       v
                DLL / Directory
                   Analysis
                       |
                       v
             Identify Misconfiguration
                       |
                       v
              Potential PrivEsc Path
```

> **Core OSCP rule:** When you find a service, don't stop at its name. Always map **`Service → Account → Binary → Path → Permissions → Configuration`**. The privilege-escalation opportunity usually appears when a **privileged service intersects with something your current user can modify**.
