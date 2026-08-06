# Windows Important Commands (Part 4)

> Windows provides several built-in commands to manage files, search for important information, and view or modify file permissions. These commands are commonly used by system administrators, incident responders, SOC analysts, and penetration testers during system enumeration and investigation.

These notes cover:

- File and Directory Management
- Searching Files
- Searching Keywords
- File and Folder Permissions
- Changing File Ownership

---

# Table of Contents

- [1. File and Directory Management](#1-file-and-directory-management)
- [2. Searching Files](#2-searching-files)
- [3. Searching Keywords](#3-searching-keywords)
- [4. File and Folder Permissions](#4-file-and-folder-permissions)
- [5. Changing File Ownership](#5-changing-file-ownership)
- [6. Quick Revision Sheet](#6-quick-revision-sheet)
- [7. Cybersecurity Perspective](#7-cybersecurity-perspective)

---

# 1. File and Directory Management

These commands help navigate, create, modify, copy, move, and delete files and directories.

---

## dir

Lists files and folders in the current directory.

Syntax:

```cmd
dir
```

---

## dir /a

Displays all files, including hidden and system files.

Syntax:

```cmd
dir /a
```

---

## dir /s

Searches recursively through all subdirectories.

Syntax:

```cmd
dir /s
```

---

## tree

Displays the complete directory structure.

Syntax:

```cmd
tree
```

---

## cd

Changes the current working directory.

Syntax:

```cmd
cd <directory>
```

---

## copy

Copies files from one location to another.

Syntax:

```cmd
copy <source> <destination>
```

---

## move

Moves files or folders.

Syntax:

```cmd
move <source> <destination>
```

---

## ren

Renames a file or folder.

Syntax:

```cmd
ren <oldname> <newname>
```

---

## del

Deletes one or more files.

Syntax:

```cmd
del <filename>
```

---

## mkdir

Creates a new directory.

Syntax:

```cmd
mkdir <directory>
```

---

## rmdir

Removes a directory.

Syntax:

```cmd
rmdir <directory>
```

---

## type

Displays the contents of a text file.

Syntax:

```cmd
type <file>
```

---

## more

Displays a text file one page at a time.

Syntax:

```cmd
more <file>
```

---

## fc

Compares two files.

Syntax:

```cmd
fc <file1> <file2>
```

---

# 2. Searching Files

Windows provides commands to search for files throughout the file system.

---

## where

Searches for an executable in the system PATH.

Syntax:

```cmd
where <program>
```

---

## dir *.txt /s

Searches recursively for all text files.

Syntax:

```cmd
dir *.txt /s
```

---

## dir *.config /s

Searches recursively for configuration files.

Syntax:

```cmd
dir *.config /s
```

---

# 3. Searching Keywords

The `findstr` command searches for specific words or patterns inside files.

---

## findstr "password" <filename>

Searches for a keyword inside a file.

Syntax:

```cmd
findstr "password" file.txt
```

---

## findstr "keyword" *.txt

Searches inside multiple text files.

Syntax:

```cmd
findstr "keyword" *.txt
```

---

## findstr /S "keyword" *.*

Searches recursively inside all files.

Syntax:

```cmd
findstr /S "keyword" *.*
```

---

## findstr /I "admin" user.txt

Performs a case-insensitive search.

Syntax:

```cmd
findstr /I "admin" user.txt
```

---

## findstr /N "keyword" file.txt

Displays matching line numbers.

Syntax:

```cmd
findstr /N "keyword" file.txt
```

---

## findstr "keyword1 keyword2" file.txt

Searches for multiple keywords.

Syntax:

```cmd
findstr "keyword1 keyword2" file.txt
```

---

## findstr /R "admin.*password" file.txt

Searches using Regular Expressions.

Syntax:

```cmd
findstr /R "admin.*password" file.txt
```

---

## netstat -ano | findstr LISTEN

Searches for listening network ports.

Syntax:

```cmd
netstat -ano | findstr LISTEN
```

---

## findstr /S /I "password" *.config

Recursively searches configuration files.

Syntax:

```cmd
findstr /S /I "password" *.config
```

---

# 4. File and Folder Permissions

The `icacls` command is used to view and manage NTFS permissions.

---

## icacls file.txt

Displays file permissions.

Syntax:

```cmd
icacls file.txt
```

Permission meanings:

```text
(F)  → Full Control

(M)  → Modify

(RX) → Read & Execute

(R)  → Read

(W)  → Write
```

---

## icacls file.txt /grant <username>:F

Grants Full Control to a user.

Syntax:

```cmd
icacls file.txt /grant User:F
```

---

## icacls file.txt /remove <username>

Removes permissions for a user.

Syntax:

```cmd
icacls file.txt /remove User
```

---

## icacls file.txt /reset

Resets permissions to their default values.

Syntax:

```cmd
icacls file.txt /reset
```

---

## icacls Folder /grant <username>:F /T

Applies permissions recursively to all files and folders.

Syntax:

```cmd
icacls Folder /grant User:F /T
```

---

# 5. Changing File Ownership

The `icacls` command can also change ownership of files and folders.

---

## icacls <file/folder> /setowner <username>

Changes the owner to a local user.

Syntax:

```cmd
icacls file.txt /setowner User
```

---

## icacls <file/folder> /setowner <domain>\<user>

Changes the owner to a domain user.

Syntax:

```cmd
icacls file.txt /setowner CONTOSO\User
```

---

# 6. Quick Revision Sheet

## File Management

```cmd
dir
dir /a
dir /s
tree
cd
copy
move
ren
del
mkdir
rmdir
type
more
fc
```

---

## Searching Files

```cmd
where <program>

dir *.txt /s

dir *.config /s
```

---

## Searching Keywords

```cmd
findstr "password" file.txt

findstr "keyword" *.txt

findstr /S "keyword" *.*

findstr /I "admin" user.txt

findstr /N "keyword" file.txt

findstr /R "admin.*password" file.txt

netstat -ano | findstr LISTEN

findstr /S /I "password" *.config
```

---

## File Permissions

```cmd
icacls file.txt

icacls file.txt /grant User:F

icacls file.txt /remove User

icacls file.txt /reset

icacls Folder /grant User:F /T
```

---

## Changing Ownership

```cmd
icacls file.txt /setowner User

icacls file.txt /setowner DOMAIN\User
```

---

# 7. Cybersecurity Perspective

These commands are frequently used during Windows enumeration, incident response, and penetration testing.

Common use cases include:

- Enumerating files and directories.
- Searching for configuration files containing sensitive information.
- Finding hardcoded credentials using `findstr`.
- Identifying listening ports with `netstat`.
- Reviewing NTFS file permissions using `icacls`.
- Detecting insecure file permissions that may lead to privilege escalation.
- Verifying ownership of sensitive files and directories.
- Auditing shared folders and important configuration files during security assessments.

---

*End of Part 4*