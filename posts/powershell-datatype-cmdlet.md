# Windows PowerShell — Data Types & Cmdlets

> **PowerShell is an object-oriented scripting language**, which means every piece of data stored in a variable has a specific **data type**. Data types determine what kind of values a variable can hold and what operations can be performed on it.
>
> PowerShell performs most operations using **Cmdlets (Command-lets)**. Cmdlets are lightweight built-in commands that follow the **Verb-Noun** naming convention.

These notes cover:

- What are Data Types
- Why Data Types are Important
- Common PowerShell Data Types
- Type Casting
- Checking Data Types
- What are Cmdlets
- Cmdlet Naming Convention
- Common Cmdlets
- Finding Cmdlets
- Getting Help for Cmdlets
- Cmdlet Parameters
- Cybersecurity Perspective
- Quick Revision Sheet

---

# Table of Contents

- [1. What are Data Types](#1-what-are-data-types)
- [2. Why Data Types are Important](#2-why-data-types-are-important)
- [3. Common PowerShell Data Types](#3-common-powershell-data-types)
- [4. Type Casting](#4-type-casting)
- [5. Checking Data Types](#5-checking-data-types)
- [6. What are Cmdlets](#6-what-are-cmdlets)
- [7. Cmdlet Naming Convention](#7-cmdlet-naming-convention)
- [8. Common Cmdlets](#8-common-cmdlets)
- [9. Finding Cmdlets](#9-finding-cmdlets)
- [10. Getting Help for Cmdlets](#10-getting-help-for-cmdlets)
- [11. Cmdlet Parameters](#11-cmdlet-parameters)
- [12. Cybersecurity Perspective](#12-cybersecurity-perspective)
- [13. Quick Revision Sheet](#13-quick-revision-sheet)

---

# 1. What are Data Types

A data type defines:

```text
What Kind of Data

a Variable

Can Store
```

Examples:

```text
Text

Numbers

True / False

Date

Array

Object
```

---

## Basic Concept

```text
Variable

↓

Stores Value

↓

Value Has

Data Type
```

---

# 2. Why Data Types are Important

Data types determine:

```text
Storage

Operations

Memory Usage

Available Methods
```

Example:

```powershell
$age = 22
```

PowerShell treats:

```text
22

↓

Integer
```

Whereas:

```powershell
$name = "Alice"
```

is treated as:

```text
String
```

---

# 3. Common PowerShell Data Types

## String

Stores text.

Example:

```powershell
$name = "Alice"
```

---

## Integer

Stores whole numbers.

Example:

```powershell
$count = 100
```

---

## Double

Stores decimal numbers.

Example:

```powershell
$price = 199.99
```

---

## Boolean

Stores:

```text
True

False
```

Example:

```powershell
$isAdmin = $true
```

---

## Array

Stores multiple values.

Example:

```powershell
$colors = @("Red","Blue","Green")
```

Access element:

```powershell
$colors[1]
```

---

## Hashtable

Stores data as:

```text
Key

↓

Value
```

Example:

```powershell
$user = @{
    Name="Alice"
    Age=22
}
```

---

## DateTime

Stores date and time.

Example:

```powershell
$date = Get-Date
```

---

## Object

PowerShell works mainly with objects.

Example:

```powershell
$process = Get-Process
```

The variable stores process objects instead of plain text.

---

## Summary Table

| Data Type | Example |
|------------|----------|
| String | `"Hello"` |
| Integer | `100` |
| Double | `15.5` |
| Boolean | `$true` |
| Array | `@(1,2,3)` |
| Hashtable | `@{}` |
| DateTime | `Get-Date` |
| Object | `Get-Process` |

---

# 4. Type Casting

Type casting converts one data type into another.

Syntax:

```powershell
[DataType]Value
```

---

## Examples

Convert String to Integer

```powershell
[int]"25"
```

Convert Integer to String

```powershell
[string]100
```

Convert String to Boolean

```powershell
[bool]"True"
```

---

## Common Cast Types

```text
[int]

[string]

[bool]

[double]

[datetime]
```

---

# 5. Checking Data Types

Use:

```powershell
GetType()
```

Example:

```powershell
$name = "Alice"

$name.GetType()
```

Output:

```text
String
```

---

Another example:

```powershell
$num = 100

$num.GetType()
```

Output:

```text
Int32
```

---

# 6. What are Cmdlets

A **Cmdlet (Command-let)** is a built-in PowerShell command.

Cmdlets are designed to perform a single task.

Examples:

```powershell
Get-Process

Get-Service

Get-Date

Get-ChildItem
```

---

## Characteristics

```text
Lightweight

Single Purpose

Built into PowerShell

Return Objects
```

---

# 7. Cmdlet Naming Convention

Cmdlets follow:

```text
Verb-Noun
```

Examples:

```powershell
Get-Process

Set-Location

New-Item

Remove-Item

Start-Service
```

---

## Common Verbs

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

# 8. Common Cmdlets

| Cmdlet | Purpose |
|---------|----------|
| Get-Help | Display Help |
| Get-Command | List Commands |
| Get-Process | Running Processes |
| Get-Service | Services |
| Get-Date | Current Date |
| Get-Location | Current Directory |
| Set-Location | Change Directory |
| Get-ChildItem | List Files/Folders |
| Get-Content | Read File |
| Get-Member | Display Object Members |

---

## Example

```powershell
Get-Process
```

Returns:

```text
Running Processes
```

---

Another example:

```powershell
Get-Service
```

Returns:

```text
Installed Services
```

---

# 9. Finding Cmdlets

Use:

```powershell
Get-Command
```

to display all available commands.

---

Find cmdlets related to processes:

```powershell
Get-Command *Process*
```

Example output:

```text
Get-Process

Stop-Process

Wait-Process

Debug-Process
```

---

# 10. Getting Help for Cmdlets

PowerShell has built-in documentation.

Useful commands:

```powershell
Get-Help

Get-Command

Get-Member
```

---

Examples:

```powershell
Get-Help Get-Process
```

Displays:

```text
Syntax

Description

Parameters

Examples
```

---

# 11. Cmdlet Parameters

Parameters modify how a cmdlet works.

General syntax:

```powershell
Cmdlet -Parameter Value
```

---

Example:

```powershell
Get-Process -Name explorer
```

Example:

```powershell
Get-Service -Name Spooler
```

---

## Benefits

Parameters allow you to:

```text
Filter Results

Specify Target

Change Behavior

Control Output
```

---

# 12. Cybersecurity Perspective

PowerShell is widely used by security professionals.

---

## Blue Team

Common tasks include:

```text
Process Investigation

Service Enumeration

Log Collection

System Auditing

Incident Response

Threat Hunting
```

---

## Red Team

PowerShell is commonly used for:

```text
System Enumeration

Automation

Remote Administration

Security Testing
```

---

## Important Cmdlets for Security

```text
Get-Process

Get-Service

Get-ChildItem

Get-Content

Get-Command

Get-Member

Get-Help
```

---

## Best Practices

```text
Understand Object Output

Verify Data Types

Use Built-in Help

Use Meaningful Variables

Prefer Cmdlets Over Aliases

Enable PowerShell Logging
```

---

# 13. Quick Revision Sheet

Data Types

```text
String

Integer

Double

Boolean

Array

Hashtable

DateTime

Object
```

---

Type Casting

```text
[int]

[string]

[bool]

[double]

[datetime]
```

---

Check Type

```powershell
GetType()
```

---

Cmdlet

```text
Built-in PowerShell Command
```

---

Naming Convention

```text
Verb-Noun
```

---

Examples

```text
Get-Process

Get-Service

New-Item

Remove-Item

Start-Service
```

---

Find Commands

```powershell
Get-Command
```

---

Get Help

```powershell
Get-Help
```

---

Biggest Concept

```text
PowerShell stores data using
different data types such as
String, Integer, Boolean,
Array, Hashtable, and Objects.

Most PowerShell operations are
performed using Cmdlets, which
are built-in commands following
the Verb-Noun naming convention.

Understanding data types and
cmdlets is essential for writing
PowerShell scripts, automating
tasks, and performing Windows
administration and cybersecurity
operations.
```

---

*End of PowerShell Data Types & Cmdlets Notes*