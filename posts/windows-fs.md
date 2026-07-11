# Windows Internals — Windows File System Overview

> The **Windows File System** defines how Windows stores, organizes, accesses, and manages files and directories on storage devices.
>
> Windows supports multiple file systems, but **NTFS (New Technology File System)** is the primary file system used by modern Windows operating systems.
>
> Understanding the Windows file system is important for **System Administration, Digital Forensics, Malware Analysis, Incident Response, and Windows Privilege Escalation**.

These notes cover:

* What is a File System
* Windows File System Types
* FAT32
* exFAT
* NTFS
* Windows Drive Structure
* Important Windows Directories
* User Profile Directory
* Important User Directories
* Program Files Directories
* ProgramData Directory
* Windows Directory
* System32 and SysWOW64
* Temporary Directories
* NTFS Permissions
* File Attributes
* Alternate Data Streams
* Important NTFS Metadata Files
* File Paths and Environment Variables
* Cybersecurity Perspective
* Quick Revision Sheet

---

# Table of Contents

* [1. What is a File System](#1-what-is-a-file-system)
* [2. Windows File System Types](#2-windows-file-system-types)
* [3. FAT32](#3-fat32)
* [4. exFAT](#4-exfat)
* [5. NTFS](#5-ntfs)
* [6. Windows Drive Structure](#6-windows-drive-structure)
* [7. Important Windows Directories](#7-important-windows-directories)
* [8. User Profile Directory](#8-user-profile-directory)
* [9. Important User Directories](#9-important-user-directories)
* [10. Program Files Directories](#10-program-files-directories)
* [11. ProgramData Directory](#11-programdata-directory)
* [12. Windows Directory](#12-windows-directory)
* [13. System32 and SysWOW64](#13-system32-and-syswow64)
* [14. Temporary Directories](#14-temporary-directories)
* [15. NTFS Permissions](#15-ntfs-permissions)
* [16. File Attributes](#16-file-attributes)
* [17. Alternate Data Streams](#17-alternate-data-streams)
* [18. Important NTFS Metadata Files](#18-important-ntfs-metadata-files)
* [19. File Paths and Environment Variables](#19-file-paths-and-environment-variables)
* [20. Cybersecurity Perspective](#20-cybersecurity-perspective)
* [21. Quick Revision Sheet](#21-quick-revision-sheet)

---

# 1. What is a File System

A file system is a method used by an operating system to:

```text
Store Data

Organize Data

Locate Data

Access Data

Manage Data
```

on storage devices.

Examples:

```text
Hard Disk

SSD

USB Drive

Memory Card
```

---

## Basic Working

```text
Application

↓

Operating System

↓

File System

↓

Storage Device
```

The application requests access to a file.

The operating system uses the file system to locate and access the data on the storage device.

---

# 2. Windows File System Types

Windows supports several file systems.

The most common are:

```text
FAT32

exFAT

NTFS
```

---

## Basic Comparison

| Feature | FAT32 | exFAT | NTFS |
|---|---|---|---|
| Security Permissions | No | No | Yes |
| Journaling | No | No | Yes |
| Compression | No | No | Yes |
| Encryption Support | No | No | Yes |
| Large File Support | Limited | Yes | Yes |
| Common Usage | Older Devices | Flash Storage | Windows System Drives |

---

# 3. FAT32

FAT32 stands for:

```text
File Allocation Table 32
```

It is an older file system.

Commonly used in:

```text
USB Drives

Memory Cards

Older Storage Devices
```

---

## Advantages

```text
Simple

Widely Supported

Compatible with Many Operating Systems
```

---

## Limitations

Maximum individual file size:

```text
4 GB
```

FAT32 does not provide:

```text
File Permissions

Journaling

Native File Encryption
```

---

# 4. exFAT

exFAT stands for:

```text
Extensible File Allocation Table
```

It was designed mainly for flash storage devices.

Commonly used in:

```text
USB Drives

SD Cards

External Storage Devices
```

---

## Advantages

```text
Supports Large Files

Good Cross-Platform Compatibility

Suitable for Flash Storage
```

Unlike FAT32:

```text
Files Larger Than 4 GB

Can Be Stored
```

---

## Limitation

exFAT does not provide the advanced security and reliability features available in NTFS.

---

# 5. NTFS

NTFS stands for:

```text
New Technology File System
```

It is the primary file system used by modern Windows installations.

---

## NTFS Provides

```text
File Permissions

Directory Permissions

Journaling

File Compression

File Encryption

Disk Quotas

Hard Links

Symbolic Links

Alternate Data Streams
```

---

## Basic Concept

```text
Windows

↓

NTFS

↓

Files and Directories

↓

Metadata

↓

Storage Device
```

NTFS stores both:

```text
User Data

and

File System Metadata
```

---

# 6. Windows Drive Structure

Windows identifies storage volumes using drive letters.

Examples:

```text
C:\

D:\

E:\
```

---

## Primary System Drive

Normally:

```text
C:\
```

contains the Windows operating system.

Basic structure:

```text
C:\

├── Windows

├── Users

├── Program Files

├── Program Files (x86)

├── ProgramData

├── PerfLogs

└── Other Files and Directories
```

---

# 7. Important Windows Directories

Some important directories are:

```text
C:\Windows

C:\Users

C:\Program Files

C:\Program Files (x86)

C:\ProgramData
```

Each directory has a different purpose.

---

# 8. User Profile Directory

User accounts normally have their own profile directory.

Location:

```text
C:\Users\<Username>
```

Example:

```text
C:\Users\Alice
```

---

## Basic Structure

```text
C:\Users\Alice

├── Desktop

├── Documents

├── Downloads

├── Pictures

├── Videos

├── Music

└── AppData
```

---

## Purpose

The user profile stores:

```text
Personal Files

Application Settings

User-Specific Configuration

Application Data
```

---

# 9. Important User Directories

## Desktop

```text
C:\Users\<Username>\Desktop
```

Contains files displayed on the user's desktop.

---

## Documents

```text
C:\Users\<Username>\Documents
```

Stores user documents and application-created files.

---

## Downloads

```text
C:\Users\<Username>\Downloads
```

Default location for downloaded files.

From a security perspective:

```text
Downloads Directory

↓

Common Initial Location

for

Downloaded Executables

Scripts

Archives

Documents
```

---

## AppData

Location:

```text
C:\Users\<Username>\AppData
```

AppData is hidden by default.

It contains application-specific data.

---

## AppData Structure

```text
AppData

├── Local

├── LocalLow

└── Roaming
```

---

## Local

```text
C:\Users\<Username>\AppData\Local
```

Stores data that normally remains on the local computer.

Examples:

```text
Application Cache

Temporary Application Data

Local Settings
```

---

## LocalLow

```text
C:\Users\<Username>\AppData\LocalLow
```

Used by applications running with lower integrity or restricted permissions.

---

## Roaming

```text
C:\Users\<Username>\AppData\Roaming
```

Stores user-specific application configuration that can support roaming user profiles in domain environments.

---

# 10. Program Files Directories

Windows commonly contains:

```text
C:\Program Files
```

and on 64-bit Windows:

```text
C:\Program Files (x86)
```

---

## Program Files

Typically stores:

```text
64-bit Applications
```

on 64-bit Windows.

---

## Program Files (x86)

Typically stores:

```text
32-bit Applications
```

on 64-bit Windows.

---

## Important Concept

```text
64-bit Windows

↓

Program Files

↓

64-bit Applications
```

```text
64-bit Windows

↓

Program Files (x86)

↓

32-bit Applications
```

---

# 11. ProgramData Directory

Location:

```text
C:\ProgramData
```

This directory is hidden by default.

It stores application data shared between users.

---

## Basic Difference

```text
AppData

↓

User-Specific Application Data
```

```text
ProgramData

↓

System-Wide / Shared Application Data
```

---

# 12. Windows Directory

Location:

```text
C:\Windows
```

This directory contains important Windows operating system files.

Examples:

```text
System Files

Executables

DLL Files

Drivers

Configuration Files

Logs

Temporary Files
```

---

## Important Subdirectories

```text
C:\Windows\System32

C:\Windows\SysWOW64

C:\Windows\Temp

C:\Windows\WinSxS

C:\Windows\Logs
```

---

# 13. System32 and SysWOW64

These directories are important in Windows.

---

## System32

Location:

```text
C:\Windows\System32
```

On modern 64-bit Windows:

```text
System32

↓

Primarily Contains

64-bit System Components
```

Examples include:

```text
cmd.exe

powershell.exe

reg.exe

services.exe
```

---

## SysWOW64

Location:

```text
C:\Windows\SysWOW64
```

On 64-bit Windows:

```text
SysWOW64

↓

Primarily Contains

32-bit System Components
```

---

## Important Concept

The naming may appear confusing.

```text
System32

↓

64-bit Components
```

```text
SysWOW64

↓

32-bit Components
```

on 64-bit Windows.

---

# 14. Temporary Directories

Windows and applications use temporary directories to store temporary data.

Common locations:

```text
C:\Windows\Temp
```

and:

```text
C:\Users\<Username>\AppData\Local\Temp
```

---

## Basic Purpose

Temporary directories may contain:

```text
Temporary Files

Installer Files

Application Cache

Extracted Files

Logs
```

---

# 15. NTFS Permissions

NTFS supports permissions for controlling access to files and directories.

Common permissions include:

```text
Full Control

Modify

Read & Execute

List Folder Contents

Read

Write
```

---

## Basic Working

```text
User / Group

↓

Attempts to Access File

↓

Windows Checks Permissions

↓

Allow

or

Deny
```

---

## Permission Inheritance

Files and subdirectories can inherit permissions from their parent directory.

```text
Parent Directory

↓

Permissions

↓

Child Directory

↓

Files
```

---

## Important Concept

Permissions determine:

```text
Who Can Access

What Resource

and

What Actions They Can Perform
```

---

# 16. File Attributes

Windows files and directories can have attributes.

Common attributes:

```text
Read-Only

Hidden

System

Archive
```

---

## Read-Only

Indicates that a file should not normally be modified.

---

## Hidden

Causes a file or directory to be hidden from normal directory views.

---

## System

Indicates that the file is associated with operating system functionality.

---

## Archive

Used to indicate that a file has changed and may need to be backed up.

---

## Important Concept

File attributes are different from NTFS permissions.

```text
Attributes

↓

Describe File Characteristics
```

```text
Permissions

↓

Control Access to Files
```

---

# 17. Alternate Data Streams

NTFS supports:

```text
Alternate Data Streams

or

ADS
```

ADS allows additional data streams to be associated with a file.

---

## Normal File

```text
report.txt
```

Main data stream:

```text
report.txt

↓

Main File Content
```

---

## File with Alternate Data Stream

Conceptually:

```text
report.txt

├── Main Data Stream

└── Additional Named Data Stream
```

The additional stream is associated with the same file.

---

## Important Concept

```text
NTFS File

can contain

More Than One Data Stream
```

Alternate Data Streams have legitimate uses but are also important during forensic and malware investigations.

---

# 18. Important NTFS Metadata Files

NTFS uses special metadata files to manage the file system.

Important examples include:

```text
$MFT

$LogFile

$Bitmap

$Boot

$Secure

$UsnJrnl
```

---

## $MFT

MFT stands for:

```text
Master File Table
```

The MFT is one of the most important structures in NTFS.

It stores records describing files and directories.

Information may include:

```text
File Name

File Size

Timestamps

Permissions

Data Location

File Attributes
```

---

## Basic Concept

```text
File Created

↓

NTFS Creates MFT Record

↓

Metadata Stored

↓

File System Can Locate
and Manage the File
```

---

## $LogFile

Used by NTFS journaling.

It records file system transactions to help maintain file system consistency after crashes or unexpected shutdowns.

---

## $Bitmap

Tracks:

```text
Used Disk Clusters

and

Available Disk Clusters
```

---

## $Boot

Contains information required to describe and access the NTFS volume.

---

## $Secure

Stores security descriptors used by files and directories.

---

## $UsnJrnl

USN stands for:

```text
Update Sequence Number
```

The USN Journal records changes made to files and directories on an NTFS volume when the journal is enabled.

Examples:

```text
File Created

File Deleted

File Renamed

File Modified
```

---

# 19. File Paths and Environment Variables

Windows uses backslashes in file paths.

Example:

```text
C:\Users\Alice\Documents\report.txt
```

---

## Absolute Path

An absolute path specifies the complete location of a file.

```text
C:\Users\Alice\Documents\report.txt
```

---

## Relative Path

A relative path specifies a location relative to the current directory.

Example:

```text
Documents\report.txt
```

---

## Important Environment Variables

Windows provides environment variables representing common directories.

Examples:

```text
%SystemRoot%

%USERPROFILE%

%TEMP%

%APPDATA%

%LOCALAPPDATA%

%PROGRAMDATA%
```

---

## Examples

```text
%SystemRoot%

↓

C:\Windows
```

```text
%USERPROFILE%

↓

C:\Users\<Username>
```

```text
%APPDATA%

↓

C:\Users\<Username>\AppData\Roaming
```

```text
%LOCALAPPDATA%

↓

C:\Users\<Username>\AppData\Local
```

---

# 20. Cybersecurity Perspective

Understanding the Windows file system is important for security professionals because security-relevant artifacts are stored throughout the file system.

---

## User Directories

Important locations include:

```text
Desktop

Documents

Downloads

AppData
```

These directories may contain:

```text
Downloaded Files

User-Created Files

Application Data

Configuration Files

Security Investigation Artifacts
```

---

## AppData Abuse

Applications legitimately store files inside AppData.

Malware may also place files in:

```text
AppData\Local

AppData\Roaming
```

because these locations are writable by the user and commonly contain application files.

---

## Temporary Directory Abuse

Temporary directories may be used to store:

```text
Downloaded Payloads

Extracted Files

Temporary Scripts

Intermediate Files
```

Security analysts should examine unusual files and execution activity originating from temporary directories.

---

## NTFS Permission Misconfigurations

Weak file or directory permissions can create security problems.

Example:

```text
Low-Privilege User

↓

Can Modify File

↓

File Used by
Privileged Process

↓

Potential Privilege Escalation Risk
```

Important permissions to investigate include:

```text
Write

Modify

Full Control
```

---

## Alternate Data Streams

Attackers may misuse Alternate Data Streams to store additional content associated with files.

Therefore, security investigations may examine files for unexpected:

```text
Named Data Streams
```

---

## MFT Forensics

The Master File Table is extremely valuable during forensic investigations.

Analysts may use MFT information to investigate:

```text
File Creation

File Deletion

File Names

File Paths

File Timestamps

File System Activity
```

---

## USN Journal Forensics

The USN Journal can help identify file system changes.

Examples:

```text
File Created

↓

File Modified

↓

File Renamed

↓

File Deleted
```

This can help reconstruct activity during incident response investigations.

---

## Suspicious Execution Locations

Security analysts commonly investigate executable files launched from unusual user-writable locations.

Examples:

```text
Downloads

AppData

Temp

ProgramData
```

The presence of an executable in these locations is not automatically malicious.

The important concept is:

```text
File Location

+

Execution Behavior

+

Parent Process

+

Network Activity

+

Persistence Mechanism

↓

Security Context
```

---

## Important Security Concept

```text
Windows File System Knowledge

↓

Understand Where Data is Stored

↓

Identify Important Artifacts

↓

Analyze Permissions

↓

Investigate Suspicious Files

↓

Reconstruct File Activity

↓

Detect Misconfigurations
and Malicious Behavior
```

---

# 21. Quick Revision Sheet

File System:

```text
Method Used by Operating System

to

Store

Organize

Locate

Access

Manage Data
```

---

Common Windows File Systems:

```text
FAT32

exFAT

NTFS
```

---

Primary Windows File System:

```text
NTFS
```

---

Important Directories:

```text
C:\Windows

C:\Users

C:\Program Files

C:\Program Files (x86)

C:\ProgramData
```

---

User Profile:

```text
C:\Users\<Username>
```

---

AppData:

```text
Local

↓

Local Application Data


LocalLow

↓

Lower-Integrity Application Data


Roaming

↓

User-Specific Roaming Application Data
```

---

Program Files:

```text
64-bit Windows

↓

Program Files

↓

Typically 64-bit Applications
```

```text
64-bit Windows

↓

Program Files (x86)

↓

Typically 32-bit Applications
```

---

Important Windows Directories:

```text
System32

↓

Primarily 64-bit System Components


SysWOW64

↓

Primarily 32-bit System Components
```

---

NTFS Permissions:

```text
Full Control

Modify

Read & Execute

List Folder Contents

Read

Write
```

---

File Attributes:

```text
Read-Only

Hidden

System

Archive
```

---

Important NTFS Metadata:

```text
$MFT

↓

Master File Table


$LogFile

↓

NTFS Transaction Journal


$Bitmap

↓

Tracks Used and Free Clusters


$Secure

↓

Security Descriptors


$UsnJrnl

↓

Records File System Changes
```

---

Important Security Locations:

```text
Downloads

AppData

Temp

ProgramData
```

---

Biggest Concept:

```text
The Windows file system controls
how files and directories are stored,
organized, accessed, and protected.

Modern Windows systems primarily use
NTFS, which provides permissions,
journaling, compression, encryption
support, and advanced features such
as Alternate Data Streams.

Important Windows directories include
Windows, Users, Program Files,
ProgramData, AppData, System32,
SysWOW64, and temporary directories.

From a cybersecurity perspective,
understanding the Windows file system
helps analysts investigate suspicious
files, analyze permissions, identify
persistence locations, examine NTFS
metadata, and reconstruct file system
activity during incident response and
digital forensic investigations.
```

---

*End of Windows File System Overview Notes*