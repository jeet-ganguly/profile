# Windows PowerShell — Basics

> **PowerShell** is Microsoft's powerful command-line shell and scripting language built on the **.NET Framework (.NET Core/.NET)**. It combines the functionality of a traditional command-line interface with an object-oriented scripting language, making it ideal for **system administration, automation, incident response, and cybersecurity tasks.**
>
> Unlike CMD, PowerShell works with **objects instead of plain text**, allowing administrators and security professionals to easily filter, sort, and manipulate system information.

These notes cover:

- What is PowerShell
- Why PowerShell is Used
- PowerShell Versions
- PowerShell Console
- PowerShell Syntax
- Cmdlets
- Aliases
- Variables
- Pipeline
- Objects
- Help System
- Execution Policy
- Profiles
- Common PowerShell Commands
- Cybersecurity Perspective
- Quick Revision Sheet

---

# Table of Contents

- [1. What is PowerShell](#1-what-is-powershell)
- [2. Why Use PowerShell](#2-why-use-powershell)
- [3. PowerShell Versions](#3-powershell-versions)
- [4. PowerShell Console](#4-powershell-console)
- [5. PowerShell Syntax](#5-powershell-syntax)
- [6. Cmdlets](#6-cmdlets)
- [7. Aliases](#7-aliases)
- [8. Variables](#8-variables)
- [9. Pipeline](#9-pipeline)
- [10. Objects](#10-objects)
- [11. Help System](#11-help-system)
- [12. Execution Policy](#12-execution-policy)
- [13. PowerShell Profiles](#13-powershell-profiles)
- [14. Common PowerShell Commands](#14-common-powershell-commands)
- [15. Cybersecurity Perspective](#15-cybersecurity-perspective)
- [16. Quick Revision Sheet](#16-quick-revision-sheet)

---

# 1. What is PowerShell

PowerShell is Microsoft's command-line shell and scripting language.

It is designed for:

```text
System Administration

Automation

Configuration Management

Task Scheduling

Remote Administration
```

Unlike Command Prompt:

```text
CMD

↓

Works with Text
```

PowerShell works with:

```text
Objects
```

This makes data processing much easier.

---

## PowerShell Architecture

```text
User

↓

PowerShell

↓

.NET Runtime

↓

Windows APIs

↓

Operating System
```

---

# 2. Why Use PowerShell

PowerShell allows administrators to automate repetitive tasks.

It is commonly used for:

```text
Managing Users

Managing Processes

Managing Services

Managing Files

Registry Management

Event Log Analysis

Network Configuration

Automation Scripts
```

---

## Advantages

```text
Object-Oriented

Powerful Automation

Built-in Scripting

Remote Management

Easy Filtering

Cross Platform (PowerShell 7)
```

---

# 3. PowerShell Versions

PowerShell has evolved over time.

---

## Windows PowerShell

Built on:

```text
.NET Framework
```

Runs only on:

```text
Windows
```

Common versions:

```text
2.0

3.0

4.0

5.1
```

---

## PowerShell (Core)

Built on:

```text
.NET
```

Runs on:

```text
Windows

Linux

macOS
```

Common versions:

```text
7.x
```

---

# 4. PowerShell Console

PowerShell can be opened using:

```text
Windows Terminal

PowerShell Console

PowerShell ISE

Visual Studio Code
```

---

## Interactive Shell

PowerShell provides an interactive shell where commands execute immediately.

Example:

```powershell
Get-Date
```

Output:

```text
Current Date and Time
```

---

# 5. PowerShell Syntax

Most PowerShell commands follow the format:

```text
Verb-Noun
```

Examples:

```powershell
Get-Process

Get-Service

Start-Service

Stop-Service

New-Item

Remove-Item
```

---

## Approved Verbs

Common verbs include:

```text
Get

Set

New

Remove

Start

Stop

Restart

Test

Clear

Copy

Move
```

---

## Naming Example

```powershell
Get-Process
```

Meaning:

```text
Verb

↓

Get

Noun

↓

Process
```

---

# 6. Cmdlets

A **Cmdlet (Command-let)** is a built-in PowerShell command.

Cmdlets are lightweight commands written specifically for PowerShell.

Examples:

```powershell
Get-Help

Get-Date

Get-Process

Get-Service

Get-ChildItem
```

---

## Cmdlet Characteristics

```text
Small

Single Purpose

Object Output

Easy to Combine
```

---

# 7. Aliases

Aliases are short names for longer commands.

Example:

| Alias | Original Cmdlet |
|--------|-----------------|
| ls | Get-ChildItem |
| dir | Get-ChildItem |
| cd | Set-Location |
| pwd | Get-Location |
| cat | Get-Content |
| ps | Get-Process |

---

## Example

Instead of:

```powershell
Get-ChildItem
```

you can use:

```powershell
ls
```

Both produce the same result.

---

# 8. Variables

Variables store information in memory.

Variables begin with:

```text
$
```

Example:

```powershell
$name = "Alice"
```

---

## Examples

```powershell
$age = 22

$city = "Delhi"

$status = $true
```

---

## Display Variable

```powershell
$name
```

Output:

```text
Alice
```

---

# 9. Pipeline

The pipeline is one of PowerShell's most powerful features.

Operator:

```text
|
```

Output from one command becomes input to another.

---

## Working

```text
Command 1

↓

Pipeline

↓

Command 2
```

---

## Example

```powershell
Get-Process | Sort-Object CPU
```

Meaning:

```text
Get Processes

↓

Sort by CPU Usage
```

---

# 10. Objects

Unlike CMD, PowerShell passes objects through the pipeline.

An object contains:

```text
Properties

Methods
```

---

## Example

A process object contains:

```text
Process Name

PID

CPU Time

Memory Usage

Start Time
```

---

## Concept

```text
Object

↓

Properties

+

Methods
```

---

# 11. Help System

PowerShell includes a built-in help system.

Useful cmdlets include:

```powershell
Get-Help

Get-Command

Get-Member
```

---

## Purpose

```text
Learn Cmdlets

View Syntax

Understand Parameters

Explore Objects
```

---

# 12. Execution Policy

Execution Policy controls whether PowerShell scripts can run.

It helps prevent accidental execution of untrusted scripts.

---

## Common Policies

| Policy | Description |
|---------|-------------|
| Restricted | No scripts allowed |
| RemoteSigned | Local scripts allowed, downloaded scripts must be signed |
| AllSigned | All scripts must be digitally signed |
| Unrestricted | All scripts can run (warning shown) |
| Bypass | No restrictions |

---

## Purpose

```text
Reduce Risk

Control Script Execution
```

---

# 13. PowerShell Profiles

A PowerShell profile is a script that runs automatically whenever PowerShell starts.

It is commonly used to:

```text
Set Variables

Create Aliases

Load Modules

Customize Environment
```

---

# 14. Common PowerShell Commands

Some commonly used cmdlets are:

| Cmdlet | Purpose |
|----------|---------|
| Get-Help | Display help |
| Get-Command | List available commands |
| Get-Process | View running processes |
| Get-Service | View services |
| Get-Date | Display date and time |
| Get-Location | Show current directory |
| Set-Location | Change directory |
| Get-ChildItem | List files and folders |
| Get-Content | Read file contents |
| Get-Member | Display object properties and methods |

---

# 15. Cybersecurity Perspective

PowerShell is one of the most important tools for both defenders and attackers.

---

## Blue Team Uses

Security teams use PowerShell for:

```text
Log Collection

System Auditing

Event Log Analysis

Threat Hunting

Process Investigation

Automation

Incident Response
```

---

## Red Team Uses

Attack simulation teams may use PowerShell for:

```text
Enumeration

Privilege Discovery

Automation

Remote Administration

System Information Collection
```

---

## Logging

Modern Windows environments can log PowerShell activity using:

```text
PowerShell Logs

Script Block Logging

Module Logging

Transcription Logging
```

These logs help defenders investigate suspicious PowerShell activity.

---

## Security Best Practices

```text
Use Latest PowerShell Version

Enable Logging

Restrict Script Execution

Use Least Privilege

Monitor Suspicious PowerShell Activity

Digitally Sign Trusted Scripts
```

---

# 16. Quick Revision Sheet

PowerShell:

```text
Microsoft's

Command-Line Shell

+

Scripting Language
```

---

Main Features:

```text
Automation

Object-Oriented

Remote Management

Task Automation

Administration
```

---

Command Format:

```text
Verb-Noun
```

Example:

```text
Get-Process
```

---

Cmdlets:

```text
Built-in Commands
```

---

Variables:

```text
Begin with

$
```

---

Pipeline:

```text
|

Passes Objects

Between Commands
```

---

Objects:

```text
Contain

Properties

Methods
```

---

Execution Policy:

```text
Controls

Script Execution
```

---

Important Cmdlets:

```text
Get-Help

Get-Command

Get-Process

Get-Service

Get-Date

Get-ChildItem

Get-Content

Get-Member
```

---

Biggest Concept:

```text
PowerShell is an object-oriented
command-line shell and scripting
language used for automation,
system administration, and
security operations.

Unlike CMD, PowerShell works
with objects instead of plain
text, making it much more
powerful for managing Windows
systems and performing
cybersecurity tasks.
```

---

*End of PowerShell Basics Notes*