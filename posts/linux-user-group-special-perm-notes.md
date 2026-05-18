# Linux User Management & Special Permissions Notes

> Linux user management and special permissions are essential for system administration, access control, multi-user environments, and security.

This includes:

* User creation and management
* Group administration
* User locking/unlocking
* SUID
* SGID
* Sticky Bit

---

# Table of Contents

1. [Linux User Management](#1-linux-user-management)
2. [Linux Group Management](#2-linux-group-management)
3. [Important User Configuration Files](#3-important-user-configuration-files)
4. [SUID (Set User ID)](#4-suid-set-user-id)
5. [SGID (Set Group ID)](#5-sgid-set-group-id)
6. [Sticky Bit](#6-sticky-bit)
7. [Practical Multi-user Scenario](#7-practical-multi-user-scenario)
8. [Permission Symbols Summary](#8-permission-symbols-summary)

---

# 1. Linux User Management

Creating and managing users is a fundamental Linux administration task.

---

## a) Create User (Basic)

```bash
useradd <username>
```

Example:

```bash
useradd jeet
```

---

### Default Behavior

When using:

```bash
useradd jeet
```

Linux:

* creates the user
* creates a group with same name
* adds user to that group

---

## b) Create User with Custom Options

```bash
useradd -g <group-name> \
-s /bin/bash \
-c "description" \
-m \
-d /home/<username> \
<username>
```

Example:

```bash
useradd -g developers \
-s /bin/bash \
-c "Security Team User" \
-m \
-d /home/jeet \
jeet
```

---

## Meaning of Options

| Option | Meaning                |
| ------ | ---------------------- |
| `-g`   | Default group          |
| `-s`   | Login shell            |
| `-c`   | Description/comment    |
| `-m`   | Create home directory  |
| `-d`   | Specify home directory |

---

## c) Delete User Only

```bash
userdel <username>
```

Example:

```bash
userdel jeet
```

Removes:

* account

Does NOT remove:

* home directory
* user files

---

## d) Delete User and Home Directory

```bash
userdel -r <username>
```

Example:

```bash
userdel -r jeet
```

---

## e) Force Delete Logged-in User

```bash
userdel -f <username>
```

Example:

```bash
userdel -f jeet
```

---

# 2. Linux Group Management

---

## a) Create Group

```bash
groupadd <group-name>
```

Example:

```bash
groupadd developers
```

---

## b) Add User to Additional Group

Default group remains unchanged:

```bash
usermod -G <group-name> <username>
```

Example:

```bash
usermod -G developers jeet
```

---

## c) Change User Default Group

```bash
usermod -g <group-name> <username>
```

Example:

```bash
usermod -g developers jeet
```

---

Difference:

| Command | Purpose                |
| ------- | ---------------------- |
| `-G`    | Secondary group        |
| `-g`    | Default(primary) group |

---

## d) Move User Home Directory

```bash
usermod -m -d /home/<new-folder> <username>
```

Example:

```bash
usermod -m -d /home/security jeet
```

---

## e) Change User Shell

```bash
usermod -s <shell-path> <username>
```

Example:

```bash
usermod -s /bin/bash jeet
```

---

## f) Lock User Account

```bash
usermod -L <username>
```

Example:

```bash
usermod -L jeet
```

Locks login access.

---

## g) Unlock User Account

```bash
usermod -U <username>
```

Example:

```bash
usermod -U jeet
```

---

# 3. Important User Configuration Files

---

## `/etc/group`

Contains:

* created groups
* group members

---

## `/etc/passwd`

Contains:

* User ID (UID)
* Group ID (GID)
* home directory
* shell type
* description

Example:

```text
jeet:x:1001:1001:Jeet:/home/jeet:/bin/bash
```

---

## `/etc/shadow`

Contains:

* password hashes
* password expiry
* last password change
* aging information

---

# 4. SUID (Set User ID)

## What is SUID?

SUID is a special permission.

When set on an executable:

> Program executes with permissions of file owner rather than the user running it.

---

Example:

```bash
passwd
```

Normal users can change passwords because:

```bash
passwd
```

has SUID enabled.

---

## Permission Appearance

Shows:

```text
s
```

or:

```text
S
```

Example:

```bash
-rwsr-xr-x
```

Notice:

```text
s
```

replaces execute bit.

---

## Set SUID

```bash
chmod u+s <filename>
```

---

## Remove SUID

```bash
chmod u-s <filename>
```

---

# 5. SGID (Set Group ID)

## What is SGID?

Behavior differs for files and directories.

---

## SGID on File

If SGID is applied to executable:

> program runs with group privileges of file owner.

---

## SGID on Directory

If enabled on directory:

> newly created files inherit directory group ownership.

---

Permission appears as:

```text
s
```

or:

```text
S
```

---

Example:

```bash
drwxrwsr-x
```

---

## Enable SGID

```bash
chmod g+s <filename>
```

---

## Remove SGID

```bash
chmod g-s <filename>
```

---

# 6. Sticky Bit

Sticky Bit is also called:

```text
Restricted Deletion Flag
```

---

When enabled on a directory:

Only:

* file owner
* directory owner
* root

can delete or rename files.

---

Permission appears as:

```text
t
```

or:

```text
T
```

---

Example:

```bash
drwxrwxrwt
```

---

## Enable Sticky Bit

```bash
chmod o+t <folder>
```

---

## Disable Sticky Bit

```bash
chmod o-t <folder>
```

---

# How to Identify Sticky Bit

```bash
ls -ld <folder>
```

Output:

```bash
drwxrwxrwt
```

Notice:

```text
t
```

at end.

---

# Common Real Example

```bash
/tmp
```

typically has:

```bash
drwxrwxrwt
```

because multiple users write there.

---

# 7. Practical Multi-user Scenario

Suppose:

Directory:

```bash
/shared
```

has:

* SGID enabled
* multiple users have write permission

Users:

```text
Jeet
Pritam
```

Both belong to same group.

---

Jeet creates:

```text
report.txt
```

inside:

```text
/shared
```

Because SGID is enabled:

```text
report.txt
```

inherits group ownership.

---

Without Sticky Bit:

Pritam may delete:

```text
report.txt
```

because he also has write permission.

---

If Sticky Bit is enabled:

```bash
chmod o+t /shared
```

Pritam cannot delete Jeet's file.

---

# 8. Permission Symbols Summary

| Permission | Symbol |
| ---------- | ------ |
| SUID       | `s`    |
| SGID       | `s`    |
| Sticky Bit | `t`    |

---

# Quick Commands Summary

```bash
useradd jeet
userdel jeet
userdel -r jeet

groupadd developers

usermod -G developers jeet
usermod -g developers jeet

usermod -L jeet
usermod -U jeet

chmod u+s file
chmod g+s file
chmod o+t folder
```

---

# Notes

* `useradd` creates user and default group.
* `-G` adds secondary group.
* `-g` changes primary group.
* SUID executes as file owner.
* SGID executes as file group or inherits group ownership.
* Sticky Bit protects files from deletion in shared directories.
* `/tmp` commonly uses Sticky Bit.

---

*End of Linux User Management & Special Permissions Notes*
