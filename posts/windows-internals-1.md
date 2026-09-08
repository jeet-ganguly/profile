# Windows Internals — Part 1

> **Focus:** User Mode, Kernel Mode, Windows Architecture, Components, and Windows API Call Flow

---

## Table of Contents

1. [User Mode vs Kernel Mode](#1-user-mode-vs-kernel-mode)
2. [Windows Architecture — High Level](#2-windows-architecture--high-level)
3. [User-Mode Components](#3-user-mode-components)
4. [Kernel-Mode Components](#4-kernel-mode-components)
5. [Important Windows Components — Summary](#5-important-windows-components--summary)
6. [Windows API Call Flow](#6-windows-api-call-flow)
7. [Example — Creating a File](#7-example--creating-a-file)
8. [Example — Process Creation](#8-example--process-creation)
9. [System Calls](#9-system-calls)
10. [Handles and Objects](#10-handles-and-objects)
11. [Security Boundary](#11-security-boundary)
12. [Security-Relevant API Flow](#12-security-relevant-api-flow)
13. [Key Mental Model](#13-key-mental-model)
14. [Security Perspective](#security-perspective)

---

# 1. User Mode vs Kernel Mode

Windows separates execution into **two primary privilege levels**:

```text
┌──────────────────────────────────────────────┐
│                 USER MODE                    │
│                                              │
│  Applications                               │
│  DLLs / Runtime Libraries                   │
│  Windows Subsystems                          │
│                                              │
│  Restricted access to system resources      │
└──────────────────────┬───────────────────────┘
                       │
                 System Call
                       │
┌──────────────────────▼───────────────────────┐
│                KERNEL MODE                   │
│                                              │
│  Executive                                   │
│  Kernel                                      │
│  Device Drivers                              │
│  HAL                                         │
│                                              │
│  Full access to system resources             │
└──────────────────────────────────────────────┘
```

### User Mode

Processes normally execute in **User Mode**.

Characteristics:

* Restricted access to hardware and kernel memory.
* Cannot directly execute privileged CPU instructions.
* Cannot directly access arbitrary physical memory.
* Applications interact with Windows through APIs.
* A faulty application normally affects only its own process.

Examples:

```text
notepad.exe
powershell.exe
explorer.exe
chrome.exe
```

### Kernel Mode

Kernel-mode components operate with highly privileged access.

Characteristics:

* Can access kernel memory and hardware.
* Can execute privileged instructions.
* Manages critical operating-system resources.
* Kernel-mode bugs can cause system-wide failures such as BSOD.

Examples:

```text
ntoskrnl.exe
Device Drivers (*.sys)
```

### Security Importance

```text
User Mode
    │
    │ System Call
    ▼
Kernel Mode
```

A vulnerability that allows controlled execution to cross this boundary incorrectly can potentially result in:

```text
Low Privilege User
       ↓
Kernel-Level Execution
       ↓
SYSTEM / Kernel Privileges
```

[↑ Back to Top](#windows-internals--part-1)

---

# 2. Windows Architecture — High Level

Windows uses a **hybrid architecture** consisting primarily of User Mode and Kernel Mode components.

```text
                    WINDOWS
                       │
        ┌──────────────┴──────────────┐
        │                             │
    USER MODE                     KERNEL MODE
        │                             │
 ┌──────┴────────┐            ┌───────┴─────────┐
 │ Applications  │            │     Executive   │
 │ Win32 Apps    │            │                 │
 │ Services      │            │ Object Manager  │
 │ Subsystems    │            │ Process Manager │
 │ Runtime DLLs  │            │ Memory Manager  │
 └──────┬────────┘            │ I/O Manager     │
        │                     │ Security Ref.   │
        │                     │ Cache Manager    │
        │                     └────────┬─────────┘
        │                              │
        │                       ┌──────▼──────┐
        │                       │   Kernel    │
        │                       └──────┬──────┘
        │                              │
        │                       ┌──────▼──────┐
        │                       │   Drivers   │
        │                       └──────┬──────┘
        │                              │
        │                       ┌──────▼──────┐
        │                       │     HAL     │
        │                       └─────────────┘
        │
        └──────────── System Calls ────────────►
```

[↑ Back to Top](#windows-internals--part-1)

---

# 3. User-Mode Components

User-mode components provide applications with interfaces for interacting with Windows.

## 3.1 Applications

Programs executed by users or services.

```text
explorer.exe
cmd.exe
powershell.exe
notepad.exe
```

Typical interaction:

```text
Application
    ↓
Windows API
    ↓
System DLL
    ↓
System Call
    ↓
Kernel
```

---

## 3.2 Windows API

The Windows API provides programming interfaces used by applications.

Examples:

```text
CreateProcess()
OpenProcess()
CreateFile()
ReadFile()
WriteFile()
VirtualAlloc()
VirtualProtect()
RegOpenKeyEx()
```

Common DLLs:

```text
kernel32.dll
advapi32.dll
user32.dll
gdi32.dll
```

---

## 3.3 Native API / NTDLL

`ntdll.dll` provides the **Native API** interface used by user-mode software to interact with the Windows kernel.

Examples:

```text
NtCreateFile()
NtOpenProcess()
NtAllocateVirtualMemory()
NtProtectVirtualMemory()
NtReadVirtualMemory()
NtWriteVirtualMemory()
NtCreateSection()
```

Conceptually:

```text
Win32 API
    ↓
KernelBase.dll
    ↓
ntdll.dll
    ↓
System Call
```

---

## 3.4 Windows Services

Services are background processes/components providing system functionality.

Common areas:

```text
Networking
Authentication
Windows Update
Event Logging
Security
```

Services are important for:

* Privilege escalation
* Persistence
* Defensive monitoring
* Incident response

---

## 3.5 Environment Subsystems

Windows provides user-mode subsystems and runtime environments that support applications.

The primary modern Windows application environment is the **Win32 subsystem**.

```text
Application
     ↓
Subsystem / Runtime
     ↓
Windows API
```

[↑ Back to Top](#windows-internals--part-1)

---

# 4. Kernel-Mode Components

## 4.1 Windows Executive

The **Executive** contains major operating-system managers.

| Component                  | Responsibility                      |
| -------------------------- | ----------------------------------- |
| Object Manager             | Manages Windows objects and handles |
| Process Manager            | Processes and threads               |
| Memory Manager             | Virtual and physical memory         |
| I/O Manager                | I/O requests and drivers            |
| Security Reference Monitor | Access checks                       |
| Configuration Manager      | Registry management                 |
| Cache Manager              | File-system caching                 |
| Plug and Play Manager      | Device management                   |
| Power Manager              | Power-state management              |

---

## 4.2 Kernel

The Windows Kernel provides low-level operating-system functionality.

Responsibilities include:

* Thread scheduling
* Interrupt handling
* Exception handling
* Synchronization
* Low-level processor management
* Hardware-related operations

Conceptually:

```text
Windows Executive
       +
Windows Kernel
       +
Drivers
       +
HAL
       =
Kernel Mode
```

---

## 4.3 Device Drivers

Drivers allow Windows to communicate with hardware and other devices.

Examples:

```text
Disk drivers
Network drivers
GPU drivers
USB drivers
File-system drivers
Security drivers
```

Common driver extension:

```text
.sys
```

Drivers are security-sensitive because they execute in **Kernel Mode**.

---

## 4.4 Hardware Abstraction Layer

The **Hardware Abstraction Layer (HAL)** provides an abstraction between Windows and platform-specific hardware.

```text
Windows Kernel
      ↓
     HAL
      ↓
Hardware
```

[↑ Back to Top](#windows-internals--part-1)

---

# 5. Important Windows Components — Summary

| Component        | Mode   | Primary Purpose                      |
| ---------------- | ------ | ------------------------------------ |
| Applications     | User   | User functionality                   |
| Windows API      | User   | Application interface                |
| `kernel32.dll`   | User   | Common Win32 APIs                    |
| `advapi32.dll`   | User   | Security, Registry, Services         |
| `user32.dll`     | User   | Windows/UI functionality             |
| `KernelBase.dll` | User   | Lower-level Win32 API implementation |
| `ntdll.dll`      | User   | Native API + system-call interface   |
| Windows Services | User   | Background functionality             |
| Executive        | Kernel | Major OS managers                    |
| Kernel           | Kernel | Low-level OS functionality           |
| Drivers          | Kernel | Hardware/system interfaces           |
| HAL              | Kernel | Hardware abstraction                 |

[↑ Back to Top](#windows-internals--part-1)

---

# 6. Windows API Call Flow

A simplified API call flow:

```text
Application
     │
     │ Windows API
     ▼
kernel32.dll / advapi32.dll / user32.dll
     │
     ▼
KernelBase.dll
     │
     ▼
ntdll.dll
     │
     │ System Call
     ▼
┌───────────────────────┐
│      KERNEL MODE      │
└───────────┬───────────┘
            ▼
System Service / Executive
            │
            ▼
Kernel / Driver
            │
            ▼
Hardware / Resource
```

### Core Principle

> **User-mode code requests an operation; the kernel performs the privileged operation.**

[↑ Back to Top](#windows-internals--part-1)

---

# 7. Example — Creating a File

Suppose an application executes:

```c
CreateFileW(...)
```

Conceptual flow:

```text
Application
    │
    ▼
CreateFileW()
    │
    ▼
KernelBase.dll
    │
    ▼
NTDLL
    │
    ▼
NtCreateFile()
    │
    ▼
System Call
    │
    ▼
I/O Manager
    │
    ▼
File-System Driver
    │
    ▼
Storage Device
```

[↑ Back to Top](#windows-internals--part-1)

---

# 8. Example — Process Creation

Simplified process-creation flow:

```text
Application
    │
    ▼
CreateProcess()
    │
    ▼
KernelBase.dll
    │
    ▼
NTDLL
    │
    ▼
Native System Call
    │
    ▼
Kernel
    │
    ├── Process Manager
    ├── Memory Manager
    ├── Security Reference Monitor
    └── Object Manager
            │
            ▼
        New Process
```

[↑ Back to Top](#windows-internals--part-1)

---

# 9. System Calls

A **system call** is the controlled transition from User Mode to Kernel Mode.

```text
USER MODE
   │
   ▼
Windows API
   │
   ▼
NTDLL
   │
   ▼
SYSTEM CALL
══════════════════════
KERNEL MODE
   │
   ▼
Kernel / Executive
```

On modern 64-bit Windows, the CPU instruction commonly associated with the transition is:

```text
syscall
```

> System-call mechanisms and syscall numbers can vary by Windows version and architecture.

[↑ Back to Top](#windows-internals--part-1)

---

# 10. Handles and Objects

Windows internally represents many resources as **Objects**.

Examples:

```text
Process
Thread
File
Registry Key
Event
Mutex
Section
Token
```

User-mode applications generally interact with these resources through **handles**.

```text
Application
     │
     │ Handle
     ▼
User Process Handle Table
     │
     ▼
Kernel Object
```

Example:

```text
OpenProcess()
     ↓
Process Handle
     ↓
Kernel Process Object
```

This concept is important for Windows security.

[↑ Back to Top](#windows-internals--part-1)

---

# 11. Security Boundary

Windows security decisions frequently involve:

```text
Process
   ↓
Access Token
   ↓
Security Reference Monitor
   ↓
Access Check
   ↓
Allow / Deny
```

An access token can contain:

```text
User SID
Group SIDs
Privileges
Integrity Level
Authentication information
```

Important relationship:

```text
Process
   +
Token
   +
Handle
   +
Object
```

[↑ Back to Top](#windows-internals--part-1)

---

# 12. Security-Relevant API Flow

API calls can provide valuable information during malware and defensive analysis.

| API                       | Security Relevance           |
| ------------------------- | ---------------------------- |
| `CreateProcess()`         | Process creation             |
| `OpenProcess()`           | Access another process       |
| `VirtualAlloc()`          | Memory allocation            |
| `VirtualProtect()`        | Change memory protection     |
| `WriteProcessMemory()`    | Write to another process     |
| `CreateRemoteThread()`    | Remote thread creation       |
| `CreateFile()`            | File access                  |
| `RegOpenKeyEx()`          | Registry access              |
| `OpenSCManager()`         | Service management           |
| `AdjustTokenPrivileges()` | Token privilege modification |

Typical analysis chain:

```text
Suspicious Process
       ↓
API Calls
       ↓
NTDLL / System Calls
       ↓
Kernel Activity
       ↓
System Resource
```

[↑ Back to Top](#windows-internals--part-1)

---

# 13. Key Mental Model

Memorize this architecture:

```text
┌────────────────────────────────────┐
│            APPLICATION             │
└──────────────────┬─────────────────┘
                   │
                   ▼
          Windows / Win32 API
                   │
                   ▼
        KernelBase / System DLLs
                   │
                   ▼
                 NTDLL
                   │
                   ▼
             SYSTEM CALL
══════════════════════════════════════
              KERNEL MODE
                   │
                   ▼
              EXECUTIVE
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
     Memory       I/O       Security
     Manager     Manager     Manager
        │          │
        ▼          ▼
     Hardware / Drivers
```

### Core Concepts

```text
User Mode
    ↓
Restricted execution environment

Kernel Mode
    ↓
Privileged execution environment

Windows API
    ↓
Application-facing interface

NTDLL
    ↓
Native API / system-call interface

System Call
    ↓
User → Kernel transition

Executive
    ↓
Major OS managers

Kernel
    ↓
Low-level OS functionality

Drivers
    ↓
Hardware / specialized kernel interfaces

HAL
    ↓
Hardware abstraction
```

[↑ Back to Top](#windows-internals--part-1)

---

# Security Perspective

The most important relationship for cybersecurity is:

```text
User Process
     ↓
API
     ↓
NTDLL
     ↓
System Call
     ↓
Kernel
     ↓
Object / Resource
```

This foundation is required for understanding:

* Windows Privilege Escalation
* Process Injection
* Malware Behavior
* EDR Telemetry
* Sysmon Activity
* DFIR
* Kernel Exploitation
* Access Tokens
* Windows Privileges
* Driver-Based Attacks
* API-Based Behavioral Detection

> **Core idea:** Most Windows applications operate in User Mode and request privileged operations through controlled interfaces. Kernel Mode validates and performs those operations on behalf of the application.

[↑ Back to Top](#windows-internals--part-1)
