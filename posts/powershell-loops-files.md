# Windows PowerShell — Loops & File Handling

> **Loops** allow PowerShell to execute the same block of code multiple times until a specified condition is met.
>
> **File Handling** allows PowerShell to create, read, modify, copy, move, rename, and delete files and directories.
>
> Together, loops and file handling are essential for **automation, scripting, system administration, log analysis, and cybersecurity tasks.**

These notes cover:

- What are Loops
- Why Loops are Used
- For Loop
- Foreach Loop
- While Loop
- Do-While Loop
- Do-Until Loop
- Break and Continue
- What is File Handling
- File and Directory Operations
- Reading File Contents
- Writing to Files
- Copying, Moving and Deleting Files
- Cybersecurity Perspective
- Quick Revision Sheet

---

# Table of Contents

- [1. What are Loops](#1-what-are-loops)
- [2. Why Use Loops](#2-why-use-loops)
- [3. For Loop](#3-for-loop)
- [4. Foreach Loop](#4-foreach-loop)
- [5. While Loop](#5-while-loop)
- [6. Do-While Loop](#6-do-while-loop)
- [7. Do-Until Loop](#7-do-until-loop)
- [8. Break and Continue](#8-break-and-continue)
- [9. What is File Handling](#9-what-is-file-handling)
- [10. File and Directory Operations](#10-file-and-directory-operations)
- [11. Reading File Contents](#11-reading-file-contents)
- [12. Writing to Files](#12-writing-to-files)
- [13. Copying, Moving and Deleting Files](#13-copying-moving-and-deleting-files)
- [14. Cybersecurity Perspective](#14-cybersecurity-perspective)
- [15. Quick Revision Sheet](#15-quick-revision-sheet)

---

# 1. What are Loops

A loop is a programming structure that executes the same block of code repeatedly.

Instead of writing the same code multiple times:

```text
Statement

Statement

Statement

Statement
```

we use:

```text
Loop

↓

Repeated Execution
```

---

## Basic Working

```text
Condition

↓

True ?

↓

Execute Block

↓

Return to Condition

↓

False

↓

Exit Loop
```

---

# 2. Why Use Loops

Loops make scripts:

```text
Shorter

Faster

Reusable

Easier to Maintain
```

Common uses:

```text
Read Files

Process Users

Scan Processes

Analyze Logs

Check Services

Automation
```

---

# 3. For Loop

A **For Loop** executes a block of code a fixed number of times.

Syntax

```powershell
for (Initialization; Condition; Update)
{
    Statements
}
```

---

## Example

```powershell
for ($i=1; $i -le 5; $i++)
{
    $i
}
```

Output

```text
1
2
3
4
5
```

---

## Working

```text
Initialize

↓

Check Condition

↓

Execute

↓

Update Counter

↓

Repeat
```

---

# 4. Foreach Loop

A **Foreach Loop** processes each item in a collection.

Syntax

```powershell
foreach ($item in $collection)
{
    Statements
}
```

---

## Example

```powershell
$colors = @("Red","Blue","Green")

foreach ($color in $colors)
{
    $color
}
```

Output

```text
Red

Blue

Green
```

---

## Working

```text
Collection

↓

First Item

↓

Execute

↓

Next Item

↓

Execute

↓

Until Collection Ends
```

---

# 5. While Loop

A **While Loop** continues executing while a condition is true.

Syntax

```powershell
while (Condition)
{
    Statements
}
```

---

## Example

```powershell
$i = 1

while ($i -le 3)
{
    $i
    $i++
}
```

Output

```text
1
2
3
```

---

## Working

```text
Check Condition

↓

True

↓

Execute

↓

Update

↓

Repeat
```

---

# 6. Do-While Loop

A **Do-While Loop** executes the code at least once.

Syntax

```powershell
do
{
    Statements
}
while (Condition)
```

---

## Example

```powershell
$i = 1

do
{
    $i
    $i++
}
while ($i -le 3)
```

Output

```text
1
2
3
```

---

## Important Concept

```text
Execute First

↓

Check Condition

↓

Repeat if True
```

---

# 7. Do-Until Loop

A **Do-Until Loop** executes until the condition becomes true.

Syntax

```powershell
do
{
    Statements
}
until (Condition)
```

---

## Example

```powershell
$i = 1

do
{
    $i
    $i++
}
until ($i -gt 3)
```

Output

```text
1
2
3
```

---

## Working

```text
Execute

↓

Condition True ?

↓

No

↓

Repeat

↓

Yes

↓

Exit
```

---

# 8. Break and Continue

## Break

Stops the loop immediately.

Example

```powershell
for ($i=1;$i -le 5;$i++)
{
    if($i -eq 3)
    {
        break
    }

    $i
}
```

Output

```text
1
2
```

---

## Continue

Skips the current iteration.

Example

```powershell
for ($i=1;$i -le 5;$i++)
{
    if($i -eq 3)
    {
        continue
    }

    $i
}
```

Output

```text
1
2
4
5
```

---

## Difference

```text
Break

↓

Exit Loop
```

```text
Continue

↓

Skip Current Iteration

↓

Continue Loop
```

---

# 9. What is File Handling

File handling allows PowerShell to work with files and directories.

Common tasks include:

```text
Create

Read

Write

Copy

Move

Rename

Delete
```

---

## Basic Working

```text
Script

↓

PowerShell

↓

File System

↓

Files & Directories
```

---

# 10. File and Directory Operations

Common cmdlets include:

| Cmdlet | Purpose |
|---------|----------|
| New-Item | Create File/Folder |
| Remove-Item | Delete File/Folder |
| Rename-Item | Rename |
| Move-Item | Move |
| Copy-Item | Copy |
| Test-Path | Check Existence |

---

## Create File

```powershell
New-Item file.txt
```

---

## Create Folder

```powershell
New-Item FolderName -ItemType Directory
```

---

## Check File Exists

```powershell
Test-Path file.txt
```

Returns:

```text
True

or

False
```

---

# 11. Reading File Contents

PowerShell can read files.

Common cmdlets:

```powershell
Get-Content

Select-String
```

---

## Read File

```powershell
Get-Content file.txt
```

Displays the file contents.

---

## Search Inside File

```powershell
Select-String "Error" logfile.txt
```

Searches for matching text.

---

# 12. Writing to Files

PowerShell can write data to files.

Common cmdlets:

```powershell
Set-Content

Add-Content

Out-File
```

---

## Replace File Content

```powershell
Set-Content file.txt "Hello"
```

---

## Append to File

```powershell
Add-Content file.txt "New Line"
```

---

## Save Output

```powershell
Get-Process | Out-File process.txt
```

---

# 13. Copying, Moving and Deleting Files

## Copy File

```powershell
Copy-Item
```

Copies files or folders.

---

## Move File

```powershell
Move-Item
```

Moves files or folders.

---

## Rename File

```powershell
Rename-Item
```

Changes the file or folder name.

---

## Delete File

```powershell
Remove-Item
```

Deletes files or folders.

---

## Basic Workflow

```text
Create

↓

Read

↓

Modify

↓

Copy

↓

Move

↓

Delete
```

---

# 14. Cybersecurity Perspective

PowerShell loops and file handling are heavily used during security operations.

---

## Common Uses

```text
Analyze Log Files

Enumerate Directories

Search IOC Files

Read Configuration Files

Backup Logs

Automate Reports

Process Large Datasets
```

---

## Example Workflow

```text
Read Log File

↓

Loop Through Each Line

↓

Find Suspicious Event

↓

Generate Alert
```

---

Another example

```text
Enumerate Files

↓

Calculate Hash

↓

Compare with IOC

↓

Report Match
```

---

## Best Practices

```text
Validate File Paths

Handle Exceptions

Avoid Deleting Important Files

Use Least Privilege

Backup Critical Data

Use Loops Efficiently
```

---

# 15. Quick Revision Sheet

Loops

```text
Repeat Code Execution
```

---

Types of Loops

```text
For

Foreach

While

Do-While

Do-Until
```

---

Loop Control

```text
Break

↓

Exit Loop


Continue

↓

Skip Iteration
```

---

File Handling

```text
Create

Read

Write

Copy

Move

Rename

Delete
```

---

Important Cmdlets

```text
New-Item

Get-Content

Set-Content

Add-Content

Out-File

Copy-Item

Move-Item

Rename-Item

Remove-Item

Test-Path

Select-String
```

---

Biggest Concept

```text
Loops allow PowerShell to
repeat tasks automatically,
making scripts shorter and
more efficient.

File handling enables
PowerShell to create, read,
modify, copy, move, rename,
and delete files and folders.

Together, loops and file
handling form the foundation
of PowerShell automation,
system administration, log
analysis, and cybersecurity
scripting.
```

---

*End of PowerShell Loops & File Handling Notes*