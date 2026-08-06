# Windows Important Commands (Part 1)

> Windows provides many built-in command-line utilities that help administrators, system engineers, SOC analysts, incident responders, and penetration testers collect information about a system. These commands are useful for system enumeration, troubleshooting, user management, and security investigations.

These notes cover:

- Introduction
- Important Environment Variables
- System Enumeration Commands

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. Important Environment Variables](#2-important-environment-variables)
- [3. System Enumeration Commands](#3-system-enumeration-commands)
- [4. Quick Revision Sheet](#4-quick-revision-sheet)

---

# 1. Introduction

Windows includes many built-in commands that allow users to gather information about:

- Current user
- Operating System
- Computer name
- Environment variables
- Installed software
- Running services
- Network configuration
- Files and directories

These commands are frequently used by:

- System Administrators
- SOC Analysts
- Incident Responders
- Digital Forensics Analysts
- Penetration Testers

---

# 2. Important Environment Variables

Environment Variables are predefined variables maintained by Windows that point to important system locations and configuration values.

They allow applications and users to access common directories without specifying their full paths.

---

## %USERPROFILE%

Points to:

```text
Current user's home directory
```

Example:

```text
C:\Users\John
```

---

## %APPDATA%

Points to:

```text
Roaming AppData folder
```

Used for:

- User application settings
- Profiles
- Configuration files

---

## %LOCALAPPDATA%

Points to:

```text
Local AppData folder
```

Stores:

- Local application data
- Cache files
- Temporary application files

---

## %TEMP%

Points to:

```text
Current user's temporary directory
```

Used for:

- Temporary files
- Installer files
- Cached data

---

## %WINDIR%

Points to:

```text
Windows installation directory
```

Usually:

```text
C:\Windows
```

---

## %SYSTEMROOT%

Points to:

```text
Windows system directory
```

Usually:

```text
C:\Windows
```

---

## %PROGRAMFILES%

Points to:

```text
64-bit Program Files directory
```

Usually:

```text
C:\Program Files
```

---

## %PROGRAMFILES(x86)%

Points to:

```text
32-bit Program Files directory
```

Usually:

```text
C:\Program Files (x86)
```

---

## %PATH%

Contains:

```text
Executable search paths
```

When a command is executed without specifying its full path, Windows searches the directories listed in the PATH variable.

---

# 3. System Enumeration Commands

System Enumeration is the process of collecting information about the operating system, current user, computer configuration, and environment.

---

## hostname

Displays the computer name.

Syntax:

```powershell
hostname
```

Example Output:

```text
DESKTOP-01
```

---

## whoami

Displays the currently logged-in user.

Syntax:

```powershell
whoami
```

Example Output:

```text
desktop\administrator
```

---

## whoami /all

Displays detailed information about the current user.

Includes:

- SID
- Group Membership
- User Privileges
- Authentication Information

Syntax:

```powershell
whoami /all
```

---

## whoami /priv

Lists all privileges assigned to the current user.

Syntax:

```powershell
whoami /priv
```

Examples of privileges:

```text
SeShutdownPrivilege

SeBackupPrivilege

SeRestorePrivilege
```

---

## whoami /groups

Displays all security groups the current user belongs to.

Syntax:

```powershell
whoami /groups
```

Useful for identifying:

- Administrators
- Remote Desktop Users
- Domain Users
- Local Groups

---

## systeminfo

Displays detailed system information.

Syntax:

```powershell
systeminfo
```

Shows information such as:

- Windows Version
- OS Build
- Installed Hotfixes
- System Manufacturer
- Processor
- Installed RAM
- Boot Time

---

## ver

Displays the Windows version.

Syntax:

```powershell
ver
```

Example Output:

```text
Microsoft Windows Version 10.0.xxxxx
```

---

## echo %USERNAME%

Displays the username of the currently logged-in user.

Syntax:

```cmd
echo %USERNAME%
```

---

## echo %COMPUTERNAME%

Displays the computer name.

Syntax:

```cmd
echo %COMPUTERNAME%
```

---

## set

Displays all environment variables available in the current session.

Syntax:

```cmd
set
```

Useful for viewing:

- PATH
- TEMP
- USERPROFILE
- APPDATA
- COMPUTERNAME
- OS information

---

# 4. Quick Revision Sheet

## Important Environment Variables

```text
%USERPROFILE%         → User Home Folder

%APPDATA%             → Roaming AppData

%LOCALAPPDATA%        → Local AppData

%TEMP%                → Temporary Directory

%WINDIR%              → Windows Directory

%SYSTEMROOT%          → Windows Directory

%PROGRAMFILES%        → 64-bit Programs

%PROGRAMFILES(x86)%   → 32-bit Programs

%PATH%                → Executable Search Paths
```

---

## System Enumeration Commands

```powershell
hostname
```

Display computer name.

---

```powershell
whoami
```

Display current user.

---

```powershell
whoami /all
```

Display complete user information.

---

```powershell
whoami /priv
```

Display user privileges.

---

```powershell
whoami /groups
```

Display group memberships.

---

```powershell
systeminfo
```

Display detailed system information.

---

```powershell
ver
```

Display Windows version.

---

```cmd
echo %USERNAME%
```

Display current username.

---

```cmd
echo %COMPUTERNAME%
```

Display computer name.

---

```cmd
set
```

Display all environment variables.

---

*End of Part 1*