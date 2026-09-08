# Windows Internals — Part 2

> **Focus:** PE Structure, Processes, DLLs, Process Injection, and Malware Investigation Workflow  
> **Purpose:** Build practical Windows internals knowledge for malware analysis, DFIR, EDR analysis, and security research.

---

## Table of Contents

1. [Portable Executable (PE)](#1-portable-executable-pe)
   - [PE Structure](#11-pe-structure)
   - [Important PE Sections](#12-important-pe-sections)
   - [Why PE Sections Matter](#13-why-pe-sections-matter)
2. [Windows Processes](#2-windows-processes)
   - [Private Address Space](#21-private-address-space)
   - [Private Handle Table](#22-private-handle-table)
   - [Access Token](#23-access-token)
   - [Threads](#24-threads)
   - [Process Mental Model](#25-process-mental-model)
3. [DLLs](#3-dlls)
   - [Import Functions](#31-import-functions)
   - [Export Functions](#32-export-functions)
   - [Imports and Exports](#33-imports-and-exports)
   - [DLL Loading](#34-dll-loading)
4. [Example — Process Injection](#4-example--process-injection)
5. [Malware Analysis & Investigation Workflow](#5-malware-analysis--investigation-workflow)
6. [Key Mental Model](#6-key-mental-model)
7. [Security Perspective](#security-perspective)

---

# 1. Portable Executable (PE)

The **Portable Executable (PE)** format is the executable file format used by Windows.

Common PE files include:

```text
.exe    → Executable
.dll    → Dynamic-link library
.sys    → Kernel driver
.ocx    → ActiveX control
```

A PE file contains metadata and structures that tell Windows how to load and execute the program.

---

## 1.1 PE Structure

Simplified PE layout:

```text
┌─────────────────────────────┐
│       DOS Header            │
├─────────────────────────────┤
│       DOS Stub              │
├─────────────────────────────┤
│       PE Signature           │
├─────────────────────────────┤
│       COFF Header            │
├─────────────────────────────┤
│       Optional Header        │
├─────────────────────────────┤
│       Section Table          │
├─────────────────────────────┤
│       .text                  │
├─────────────────────────────┤
│       .rdata                 │
├─────────────────────────────┤
│       .data                  │
├─────────────────────────────┤
│       .pdata                 │
├─────────────────────────────┤
│       .rsrc                  │
├─────────────────────────────┤
│       .reloc                 │
└─────────────────────────────┘
```

### Important Headers

| Component | Purpose |
|---|---|
| DOS Header | Legacy DOS information; contains pointer to PE header |
| DOS Stub | Legacy DOS-mode program |
| PE Signature | Identifies the file as a PE |
| COFF Header | Architecture and section information |
| Optional Header | Image characteristics, entry point, memory layout |
| Section Table | Describes PE sections |

The PE signature is:

```text
PE\0\0
```

The DOS header normally begins with:

```text
MZ
```

---

# 1.2 Important PE Sections

The section names are conventions rather than absolute requirements. Malware can rename, add, remove, or modify sections.

| Section | Typical Purpose |
|---|---|
| `.text` | Executable code |
| `.rdata` | Read-only data |
| `.data` | Initialized writable data |
| `.bss` | Uninitialized data |
| `.idata` | Import information |
| `.edata` | Export information |
| `.rsrc` | Resources |
| `.reloc` | Relocation information |
| `.pdata` | Exception/unwind information on relevant PE formats |
| `.tls` | Thread Local Storage data/callbacks |

### `.text`

Contains executable machine code.

Typical permission:

```text
READ + EXECUTE
```

Security relevance:

- Main program code
- Function implementations
- Malware logic
- Potential shellcode/code regions

---

### `.rdata`

Usually contains read-only data.

Examples:

```text
Strings
Constants
Import-related structures
Read-only tables
```

Typical permission:

```text
READ
```

---

### `.data`

Contains initialized writable global/static data.

Typical permission:

```text
READ + WRITE
```

Examples:

```text
Global variables
Configuration data
Runtime state
```

---

### `.rsrc`

Contains Windows resources.

Examples:

```text
Icons
Dialogs
Version information
Manifest
Embedded files
Images
```

Malware may abuse resources to store:

```text
Embedded payloads
Encrypted configuration
Secondary executables
```

---

### `.reloc`

Contains relocation information used when an image cannot be loaded at its preferred address.

Relevant concepts:

```text
Image Base
ASLR
Relocations
```

---

### `.idata` / Import Information

Contains structures associated with imported functions.

For example:

```text
KERNEL32.dll
    └── CreateFileW
    └── CreateProcessW

ADVAPI32.dll
    └── RegOpenKeyExW
```

Imports can provide useful clues about a program's capabilities.

---

### `.edata` / Export Information

Contains information about functions or symbols exported by the PE.

Example:

```text
malware.dll
    ├── Initialize
    ├── Execute
    └── Cleanup
```

---

# 1.3 Why PE Sections Matter

During malware analysis, PE sections provide an initial picture of the binary.

For example:

```text
.text
  ↓
Executable code

.rdata
  ↓
Strings / constants

.data
  ↓
Writable global data

.rsrc
  ↓
Possible embedded resources

.idata
  ↓
Imported APIs

.edata
  ↓
Exported APIs

.reloc
  ↓
Relocation information
```

### Suspicious Indicators

Look for:

```text
Unusual section names
Executable + writable sections
Very high section entropy
Large embedded resources
Unexpected imports
Unexpected TLS callbacks
Modified section characteristics
```

> **Important:** None of these indicators alone proves that a file is malicious.

[↑ Back to Top](#windows-internals--part-2)

---

# 2. Windows Processes

A **process** is a container representing a running program.

A process provides the environment in which its threads execute.

A simplified process consists of:

```text
Process
│
├── Private Virtual Address Space
├── Handle Table
├── Access Token
├── Threads
├── Loaded Modules / DLLs
└── Process-related Kernel Objects
```

---

# 2.1 Private Address Space

Each process normally receives its own **virtual address space**.

Conceptually:

```text
Process A
┌─────────────────────────────┐
│ Private Virtual Address Space│
│                             │
│ Code                        │
│ DLLs                        │
│ Heap                        │
│ Stack(s)                    │
│ Mapped Memory               │
└─────────────────────────────┘

Process B
┌─────────────────────────────┐
│ Private Virtual Address Space│
│                             │
│ Code                        │
│ DLLs                        │
│ Heap                        │
│ Stack(s)                    │
│ Mapped Memory               │
└─────────────────────────────┘
```

The virtual address spaces are isolated from one another by the operating system and hardware memory-management mechanisms.

### Important Memory Areas

A process may contain:

```text
Image
DLLs
Heap
Thread Stacks
Memory Mappings
Private Allocations
```

---

## Private Memory

Memory can be backed privately by a process.

Common examples:

```text
Heap allocations
VirtualAlloc allocations
Private process data
```

Memory protection can include:

```text
PAGE_READONLY
PAGE_READWRITE
PAGE_EXECUTE_READ
PAGE_EXECUTE_READWRITE
```

A region that is both **writable and executable** can deserve investigation, although it is not automatically malicious.

---

# 2.2 Private Handle Table

Processes interact with many Windows objects through **handles**.

Examples:

```text
File
Process
Thread
Registry Key
Event
Mutex
Token
Section
```

Conceptually:

```text
Process
   │
   ▼
Handle Table
   │
   ├── 0x100 → File Object
   ├── 0x104 → Process Object
   ├── 0x108 → Event Object
   └── 0x10C → Token Object
```

A handle is essentially a reference that allows a process to access an object according to the granted access rights.

### Security Importance

If Process A obtains a handle to Process B with powerful access rights, Process A may be able to perform operations against Process B.

This is important when investigating:

```text
Process Injection
Process Manipulation
Credential Theft
Debugging
Security Tool Evasion
```

---

# 2.3 Access Token

An **access token** represents the security context of a process or thread.

It contains security information used during authorization decisions.

Conceptually:

```text
Access Token
│
├── User SID
├── Group SIDs
├── Privileges
├── Integrity Level
└── Other security information
```

Example:

```text
Process
   │
   ▼
Access Token
   │
   ├── User
   ├── Groups
   ├── Privileges
   └── Integrity Level
```

### Integrity Levels

Common Windows integrity levels include:

```text
Low
Medium
High
System
```

Simplified relationship:

```text
Low
 ↓
Medium
 ↓
High
 ↓
System
```

Integrity levels are one component of Windows security and are not simply equivalent to administrator membership.

---

# 2.4 Threads

A **thread** is the unit of execution scheduled by Windows.

A process can contain multiple threads.

```text
Process
│
├── Thread 1
├── Thread 2
├── Thread 3
└── Thread 4
```

Each thread has its own execution state, including:

```text
Thread ID
CPU context
Stack
Scheduling information
```

Threads share many process resources:

```text
Address Space
Loaded Modules
Handle Table
Process Token
```

### Process vs Thread

| Process | Thread |
|---|---|
| Resource container | Execution unit |
| Has virtual address space | Executes within process address space |
| Has handle table | Can use process handles |
| Has security context | May have thread-specific security context |
| Contains one or more threads | Belongs to a process |

---

# 2.5 Process Mental Model

Remember a process as:

```text
                  PROCESS
                     │
       ┌─────────────┼─────────────┐
       │             │             │
       ▼             ▼             ▼
   Address        Handles        Token
    Space         Table
       │
       ▼
   ┌───────────────┐
   │    Threads    │
   ├───────────────┤
   │ Thread 1      │
   │ Thread 2      │
   │ Thread 3      │
   └───────────────┘
```

This model is extremely useful when analyzing process injection.

[↑ Back to Top](#windows-internals--part-2)

---

# 3. DLLs

A **Dynamic-Link Library (DLL)** contains code and/or data that can be loaded and used by processes.

Examples:

```text
kernel32.dll
ntdll.dll
advapi32.dll
user32.dll
ws2_32.dll
```

Instead of placing every function inside an executable:

```text
Application
   ↓
Uses functions from DLL
```

This enables code reuse and dynamic linking.

---

# 3.1 Import Functions

An **import** represents functionality that a PE expects to obtain from another module.

Example:

```text
program.exe
    │
    └── Imports
          │
          ├── KERNEL32.dll
          │      ├── CreateFileW
          │      └── CreateProcessW
          │
          └── ADVAPI32.dll
                 └── RegOpenKeyExW
```

Imports can be inspected during static malware analysis.

### Security Value

Imports can provide clues about intended functionality.

For example:

```text
CreateProcessW()
    ↓
Process creation capability

RegSetValueExW()
    ↓
Registry modification capability

WinHttpOpen()
    ↓
HTTP communication capability
```

> An imported API indicates that the program may use that functionality; it does not prove that the function is actually called during a particular execution.

---

# 3.2 Export Functions

An **export** is a function or symbol made available by a module to other modules.

Example:

```text
example.dll
    │
    └── Exports
         ├── Initialize
         ├── Execute
         └── Cleanup
```

Another program can resolve and call an exported function.

Common APIs associated with dynamic loading include:

```text
LoadLibrary()
GetProcAddress()
```

Conceptual flow:

```text
LoadLibrary()
      ↓
Load DLL
      ↓
GetProcAddress()
      ↓
Find exported function
      ↓
Call function
```

---

# 3.3 Imports and Exports

The relationship can be visualized as:

```text
          IMPORTER
        program.exe
             │
             │ imports
             ▼
       ┌─────────────┐
       │   DLL       │
       │             │
       │   exports   │
       │   FunctionA │
       │   FunctionB │
       └─────────────┘
```

### Simple Example

```text
program.exe

Imports:
    FunctionA
    FunctionB

        ↓

example.dll

Exports:
    FunctionA
    FunctionB
    FunctionC
```

The executable can use `FunctionA` and `FunctionB`, but `FunctionC` is not necessarily used by that executable.

---

# 3.4 DLL Loading

DLLs can be loaded through different mechanisms.

Typical conceptual flow:

```text
Application
     ↓
LoadLibrary()
     ↓
Windows Loader
     ↓
Map DLL into Process Address Space
     ↓
Resolve Dependencies
     ↓
Resolve Imports
     ↓
Initialize Module
```

Once loaded:

```text
Process Address Space
│
├── Main EXE
├── ntdll.dll
├── kernel32.dll
├── kernelbase.dll
├── Other DLLs
└── Application DLLs
```

The actual loader behavior is more complex and involves the Windows loader, PE structures, module dependencies, relocations, and initialization routines.

[↑ Back to Top](#windows-internals--part-2)

---

# 4. Example — Process Injection

**Process injection** is a family of techniques where code or a module is caused to execute inside another process.

A simplified conceptual example:

```text
Attacker Process
       │
       │ Open target process
       ▼
Target Process
       │
       │ Allocate memory
       ▼
Remote Memory
       │
       │ Write payload
       ▼
Payload in Target
       │
       │ Trigger execution
       ▼
Code executes inside
Target Process
```

A classic conceptual sequence is:

```text
OpenProcess()
      ↓
VirtualAllocEx()
      ↓
WriteProcessMemory()
      ↓
CreateRemoteThread()
```

### Step-by-Step Concept

#### 1. Obtain target process handle

```text
OpenProcess()
```

The source process requests access to the target process.

```text
Source Process
      │
      ▼
Target Process Handle
```

#### 2. Allocate memory

```text
VirtualAllocEx()
```

Memory is allocated inside the target process.

```text
Target Process
┌──────────────────────┐
│ Existing Memory      │
│ Existing DLLs        │
│                      │
│ New Memory ◄─────────┤
└──────────────────────┘
```

#### 3. Write payload

```text
WriteProcessMemory()
```

Data is written into the allocated region.

```text
Source Process
      │
      │ Payload
      ▼
Target Process
      │
      └── Remote Memory
```

#### 4. Execute payload

One possible technique is:

```text
CreateRemoteThread()
```

which can create a thread in the target process to execute code.

Conceptually:

```text
Target Process
      │
      ├── Existing Thread
      ├── Existing Thread
      │
      └── Remote Thread
              ↓
          Payload Code
```

### Investigation Perspective

A suspicious combination such as:

```text
OpenProcess
     +
VirtualAllocEx
     +
WriteProcessMemory
     +
Remote execution
```

can be a strong behavioral signal for investigation.

However:

> These APIs can also be used legitimately by debuggers, accessibility tools, profilers, and other software. Context is required.

[↑ Back to Top](#windows-internals--part-2)

---

# 5. Malware Analysis & Investigation Workflow

A practical malware investigation should move from **initial triage → static analysis → behavioral analysis → deeper investigation → reporting**.

```text
                Malware Sample
                      │
                      ▼
                 1. Triage
                      │
                      ▼
               2. Static Analysis
                      │
                      ▼
             3. Behavioral Analysis
                      │
                      ▼
              4. Memory Analysis
                      │
                      ▼
             5. Network Analysis
                      │
                      ▼
              6. IOC Extraction
                      │
                      ▼
             7. MITRE ATT&CK Mapping
                      │
                      ▼
              8. Detection / Report
```

---

## 5.1 Step 1 — Initial Triage

First establish basic information.

Check:

```text
File name
File type
File size
Hash
Digital signature
Compile timestamp
Architecture
Entropy
PE characteristics
```

Generate hashes:

```text
MD5
SHA-1
SHA-256
```

Prefer SHA-256 for modern identification.

---

## 5.2 Step 2 — Static Analysis

Analyze the file without executing it.

Look at:

```text
PE Headers
Sections
Imports
Exports
Strings
Resources
TLS callbacks
Embedded files
Packing indicators
```

Important questions:

```text
Is it packed?
What APIs does it import?
Are there suspicious strings?
Does it contain URLs/IPs?
Does it contain embedded payloads?
Are the PE sections unusual?
```

---

## 5.3 Step 3 — Behavioral Analysis

Execute the sample only in an **isolated analysis environment**.

Observe:

```text
Process creation
File creation/modification
Registry activity
Service activity
DLL loading
Memory activity
Network connections
Child processes
Persistence mechanisms
```

Conceptual workflow:

```text
Malware
  │
  ├── Process Activity
  ├── File Activity
  ├── Registry Activity
  ├── Network Activity
  └── Memory Activity
```

---

## 5.4 Step 4 — Process Analysis

Investigate:

```text
Parent Process
Child Processes
Command Line
Loaded DLLs
Threads
Handles
Token
Integrity Level
Memory Regions
```

Useful questions:

```text
Who launched the process?

What processes did it create?

Which DLLs were loaded?

What unusual memory regions exist?

What privileges does the process have?
```

---

## 5.5 Step 5 — Network Analysis

Investigate:

```text
DNS queries
Destination IPs
Destination ports
HTTP/HTTPS
TLS connections
User-Agent
URI paths
DNS tunneling indicators
```

Build the relationship:

```text
Malware
   ↓
DNS
   ↓
Domain
   ↓
IP
   ↓
Connection
   ↓
C2 / External Service
```

---

## 5.6 Step 6 — Memory Analysis

Memory analysis can reveal information that is not obvious from the original file.

Look for:

```text
Injected code
Unusual executable memory
Loaded modules
Process relationships
Strings
Credentials / secrets
Network artifacts
Unpacked payloads
```

Especially investigate:

```text
RWX / unusual executable regions
Private executable memory
Memory regions not backed by normal images
Unexpected DLLs
Suspicious threads
```

---

## 5.7 Step 7 — IOC Extraction

Extract indicators such as:

```text
File hashes
File paths
Registry keys
Domains
IP addresses
URLs
Mutex names
Service names
Scheduled task names
Command lines
Dropped files
```

Example:

```text
SHA256
Domain
IP
File Path
Registry Key
Mutex
```

These can then be used for:

```text
Detection
Threat Hunting
SIEM Searches
EDR Queries
Blocking
Incident Response
```

---

## 5.8 Step 8 — MITRE ATT&CK Mapping

Map observed behavior to techniques.

Example:

```text
Observed:
PowerShell execution
        ↓
MITRE ATT&CK:
Command and Scripting Interpreter: PowerShell
```

Another example:

```text
Observed:
Remote process manipulation
        ↓
Potential ATT&CK technique:
Process Injection
```

The important workflow is:

```text
Observation
    ↓
Evidence
    ↓
Behavior
    ↓
ATT&CK Technique
```

Do **not** map based only on the presence of an API name.

---

## 5.9 Step 9 — Detection and Reporting

Final investigation output should summarize:

```text
1. What happened?
2. How did execution begin?
3. What processes were involved?
4. What files/registry locations changed?
5. What network connections occurred?
6. What persistence was established?
7. What credentials or privileges were targeted?
8. What ATT&CK techniques were observed?
9. What IOCs were identified?
10. What detections can be created?
```

A useful investigation timeline:

```text
T0
 ↓
Initial Execution
 ↓
Process Creation
 ↓
Persistence
 ↓
Payload Execution
 ↓
Network Communication
 ↓
Additional Activity
```

[↑ Back to Top](#windows-internals--part-2)

---

# 6. Key Mental Model

For Windows malware analysis, connect these concepts:

```text
                 PE FILE
                    │
                    ▼
                PROCESS
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
  Address Space   Handles      Token
       │                         │
       ▼                         ▼
   DLLs / Code              Security Context
       │
       ▼
    Threads
       │
       ▼
   API Calls
       │
       ▼
 Windows Kernel
       │
 ┌─────┼─────┬─────────┐
 ▼     ▼     ▼         ▼
File  Registry Network  Process
```

---

# 7. Security Perspective

The most important concepts from Part 2 are:

```text
PE
 ↓
How Windows represents executable files

Process
 ↓
Container for execution and resources

Address Space
 ↓
Memory available to the process

Handle Table
 ↓
References to Windows objects

Access Token
 ↓
Security identity and privileges

Thread
 ↓
Unit of execution

DLL
 ↓
Reusable code loaded into processes

Imports
 ↓
Functions requested from other modules

Exports
 ↓
Functions exposed by a module

Process Injection
 ↓
Code execution inside another process

Malware Investigation
 ↓
Static + Behavioral + Memory + Network analysis
```

### Final Workflow to Remember

```text
┌──────────────────────┐
│      PE Sample       │
└──────────┬───────────┘
           ↓
      PE Analysis
           ↓
      Imports/Exports
           ↓
       Execution
           ↓
        Process
           ↓
 ┌─────────┼─────────┐
 ↓         ↓         ↓
Memory   Threads    DLLs
 ↓         ↓         ↓
 └─────────┼─────────┘
           ↓
       API Activity
           ↓
 ┌─────────┼──────────────┐
 ↓         ↓              ↓
Files   Registry       Network
           ↓
       Investigation
           ↓
      IOCs + ATT&CK
           ↓
      Detection/Report
```

> **Core idea:** A Windows malware sample becomes a process, the process owns memory/resources and executes through threads, DLLs provide additional functionality, API activity reveals behavior, and the combination of static, process, memory, file, registry, and network evidence forms the basis of a complete investigation.

[↑ Back to Top](#windows-internals--part-2)