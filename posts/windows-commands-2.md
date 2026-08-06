# Windows Important Commands (Part 2)

> Windows provides several built-in commands to enumerate users, groups, running processes, and services. These commands are commonly used by system administrators, SOC analysts, incident responders, and penetration testers during system assessment and troubleshooting.

These notes cover:

- User and Group Enumeration
- Process Management
- Service Enumeration

---

# Table of Contents

- [1. User and Group Enumeration](#1-user-and-group-enumeration)
- [2. Process Management](#2-process-management)
- [3. Service Enumeration](#3-service-enumeration)
- [4. Quick Revision Sheet](#4-quick-revision-sheet)

---

# 1. User and Group Enumeration

User and Group Enumeration helps identify local users, groups, user privileges, and account information on a Windows system.

---

## net user

Displays all local user accounts.

Syntax:

```cmd
net user
```

Example Output:

```text
Administrator

Guest

John
```

---

## net user <username>

Displays detailed information about a specific user.

Syntax:

```cmd
net user administrator
```

Shows information such as:

- Full Name
- Password Status
- Account Active
- Group Membership
- Last Logon

---

## net localgroup

Displays all local groups available on the system.

Syntax:

```cmd
net localgroup
```

Example Output:

```text
Administrators

Users

Remote Desktop Users

Backup Operators
```

---

## net localgroup administrators

Displays members of the local Administrators group.

Syntax:

```cmd
net localgroup administrators
```

Useful for identifying users with administrative privileges.

---

## net localgroup "Remote Desktop Users"

Displays users who are allowed to access the system using Remote Desktop (RDP).

Syntax:

```cmd
net localgroup "Remote Desktop Users"
```

---

## wmic useraccount get name,sid

Displays usernames along with their Security Identifiers (SID).

Syntax:

```cmd
wmic useraccount get name,sid
```

Example Output:

```text
Administrator    S-1-5-21-...

John             S-1-5-21-...
```

---

# 2. Process Management

Process Management commands help monitor and manage running applications and background processes.

---

## tasklist

Displays all running processes.

Syntax:

```cmd
tasklist
```

Example Output:

```text
explorer.exe

chrome.exe

notepad.exe
```

---

## tasklist /svc

Displays running processes along with their associated Windows services.

Syntax:

```cmd
tasklist /svc
```

Useful for identifying which services are running inside a process.

---

## tasklist /v

Displays detailed information about running processes.

Syntax:

```cmd
tasklist /v
```

Shows additional information such as:

- User Name
- Session Name
- CPU Time
- Window Title

---

## taskkill /PID <PID> /F

Forcefully terminates a running process using its Process ID (PID).

Syntax:

```cmd
taskkill /PID 1234 /F
```

Where:

```text
/PID → Process ID

/F   → Force termination
```

---

## wmic process list brief

Displays a brief list of running processes.

Syntax:

```cmd
wmic process list brief
```

Shows basic process information.

---

## wmic process get name,processid,parentprocessid

Displays process names, Process IDs (PID), and Parent Process IDs (PPID).

Syntax:

```cmd
wmic process get name,processid,parentprocessid
```

Useful for understanding parent-child process relationships.

---

# 3. Service Enumeration

Windows services are background programs that start automatically or manually to provide system functionality.

Enumerating services helps identify installed and running services.

---

## sc query

Displays currently running services.

Syntax:

```cmd
sc query
```

---

## sc query state= all

Displays all services, including both running and stopped services.

Syntax:

```cmd
sc query state= all
```

---

## sc qc <service_name>

Displays the configuration of a specific service.

Syntax:

```cmd
sc qc WinDefend
```

Shows information such as:

- Service Type
- Binary Path
- Start Type
- Error Control

---

## sc queryex

Displays services along with their associated Process IDs (PID).

Syntax:

```cmd
sc queryex
```

Useful for mapping services to running processes.

---

## net start

Displays all currently running services.

Syntax:

```cmd
net start
```

---

## wmic service list brief

Displays a brief list of installed Windows services.

Syntax:

```cmd
wmic service list brief
```

Shows basic information such as:

- Service Name
- State
- Start Mode

---

# 4. Quick Revision Sheet

## User and Group Enumeration

```cmd
net user
```

Display all local users.

---

```cmd
net user <username>
```

Display detailed information about a specific user.

---

```cmd
net localgroup
```

Display all local groups.

---

```cmd
net localgroup administrators
```

Display members of the Administrators group.

---

```cmd
net localgroup "Remote Desktop Users"
```

Display users allowed to use Remote Desktop.

---

```cmd
wmic useraccount get name,sid
```

Display usernames and SIDs.

---

## Process Management

```cmd
tasklist
```

Display running processes.

---

```cmd
tasklist /svc
```

Display processes with associated services.

---

```cmd
tasklist /v
```

Display detailed process information.

---

```cmd
taskkill /PID <PID> /F
```

Terminate a process.

---

```cmd
wmic process list brief
```

Display a brief process list.

---

```cmd
wmic process get name,processid,parentprocessid
```

Display process names, PIDs, and Parent PIDs.

---

## Service Enumeration

```cmd
sc query
```

Display running services.

---

```cmd
sc query state= all
```

Display all services.

---

```cmd
sc qc <service_name>
```

Display service configuration.

---

```cmd
sc queryex
```

Display services with Process IDs.

---

```cmd
net start
```

Display running services.

---

```cmd
wmic service list brief
```

Display installed services.

---

*End of Part 2*