# Linux Daily Useful Commands Notes

> These are commonly used Linux commands that are useful in day-to-day work for:
>
> * System Administration
> * Cybersecurity
> * Reverse Engineering
> * OSINT
> * CTFs
> * Troubleshooting
> * General Linux usage

Learning these commands reduces dependency on GUI tools and improves efficiency.

---

# Table of Contents

1. [Navigation Commands](#1-navigation-commands)
2. [File and Directory Commands](#2-file-and-directory-commands)
3. [Viewing File Content](#3-viewing-file-content)
4. [Searching Commands](#4-searching-commands)
5. [User Information Commands](#5-user-information-commands)
6. [Process Management Commands](#6-process-management-commands)
7. [System Information Commands](#7-system-information-commands)
8. [Disk Usage Commands](#8-disk-usage-commands)
9. [Networking Commands](#9-networking-commands)
10. [Archive & Compression Commands](#10-archive--compression-commands)
11. [Permission Commands](#11-permission-commands)
12. [Useful Shortcuts and Tricks](#12-useful-shortcuts-and-tricks)

---

# 1. Navigation Commands

---

## Show Current Directory

```bash
pwd
```

Displays:

```text
Present Working Directory
```

Example:

```text
/home/jeet/Documents
```

---

## List Files

```bash
ls
```

---

## Detailed List

```bash
ls -l
```

Shows:

* permissions
* owner
* size
* date

---

## Show Hidden Files

```bash
ls -la
```

Hidden files start with:

```text
.
```

Example:

```text
.bashrc
.gitconfig
```

---

## Change Directory

```bash
cd folder_name
```

Go back:

```bash
cd ..
```

Go home:

```bash
cd ~
```

Previous directory:

```bash
cd -
```

---

# 2. File and Directory Commands

---

## Create File

```bash
touch file.txt
```

---

## Create Directory

```bash
mkdir folder
```

Create nested directories:

```bash
mkdir -p a/b/c
```

---

## Copy File

```bash
cp source destination
```

Copy folder:

```bash
cp -r folder1 folder2
```

---

## Move / Rename

```bash
mv old new
```

---

## Delete File

```bash
rm file
```

Delete folder:

```bash
rm -r folder
```

Force delete:

```bash
rm -rf folder
```

Be careful:

```bash
rm -rf
```

can permanently remove data.

---

# 3. Viewing File Content

---

## View Entire File

```bash
cat file.txt
```

---

## View Large File Page-by-Page

```bash
less file.txt
```

Navigation:

```text
Space → next page
q → quit
```

---

## View Beginning of File

```bash
head file.txt
```

Default:

```text
10 lines
```

Specify:

```bash
head -n 20 file.txt
```

---

## View End of File

```bash
tail file.txt
```

Real-time logs:

```bash
tail -f logfile.log
```

---

# 4. Searching Commands

---

## Search File by Name

```bash
find . -name file.txt
```

---

## Search Text in File

```bash
grep "word" file.txt
```

Ignore case:

```bash
grep -i "admin" file.txt
```

Recursive search:

```bash
grep -r password .
```

---

## Locate File Quickly

```bash
locate filename
```

Update database:

```bash
updatedb
```

---

## Search Command Path

```bash
which python
```

---

# 5. User Information Commands

---

## Current User

```bash
whoami
```

---

## User Identity

```bash
id
```

Shows:

* UID
* GID
* groups

---

## Logged-in Users

```bash
who
```

---

## Last Login Information

```bash
last
```

---

# 6. Process Management Commands

---

## View Running Processes

```bash
ps
```

Detailed:

```bash
ps aux
```

---

## Real-time Process View

```bash
top
```

Improved version:

```bash
htop
```

---

## Kill Process

```bash
kill PID
```

Force:

```bash
kill -9 PID
```

---

## Search Process

```bash
ps aux | grep firefox
```

---

## Background Execution

```bash
program &
```

---

# 7. System Information Commands

---

## Kernel Information

```bash
uname -a
```

---

## Linux Distribution

```bash
cat /etc/os-release
```

---

## CPU Information

```bash
lscpu
```

---

## Memory Information

```bash
free -h
```

---

## Uptime

```bash
uptime
```

---

## System Architecture

```bash
arch
```

---

# 8. Disk Usage Commands

---

## Disk Space

```bash
df -h
```

---

## Directory Size

```bash
du -sh folder
```

---

## Largest Files

```bash
du -ah | sort -rh | head
```

---

# 9. Networking Commands

---

## Test Connectivity

```bash
ping google.com
```

---

## Show IP Address

Old:

```bash
ifconfig
```

Modern:

```bash
ip a
```

---

## Routing Table

```bash
ip route
```

---

## DNS Lookup

```bash
nslookup google.com
```

---

## Open Connections

```bash
netstat -tulpn
```

Modern:

```bash
ss -tulpn
```

---

## Download File

```bash
wget URL
```

or:

```bash
curl URL
```

---

# 10. Archive & Compression Commands

---

## Create tar Archive

```bash
tar -cvf archive.tar folder
```

---

## Extract tar

```bash
tar -xvf archive.tar
```

---

## Extract gzip

```bash
tar -xzvf archive.tar.gz
```

---

## Create zip

```bash
zip file.zip data.txt
```

---

## Extract zip

```bash
unzip file.zip
```

---

# 11. Permission Commands

---

## Change Permissions

```bash
chmod 755 file
```

---

## Change Ownership

```bash
chown user file
```

---

## Change Group

```bash
chgrp group file
```

---

## Switch User

```bash
su username
```

or:

```bash
sudo su
```

---

# 12. Useful Shortcuts and Tricks

---

## Previous Command

```bash
!!
```

Runs:

```text
last executed command
```

---

## Last Argument

```bash
!$
```

Uses last argument from previous command.

Example:

```bash
mkdir test
cd !$
```

---

## Reverse History Search

Press:

```text
CTRL + R
```

Search previous commands.

---

## Clear Screen

```bash
clear
```

Shortcut:

```text
CTRL + L
```

---

## Stop Running Program

```text
CTRL + C
```

---

## Suspend Process

```text
CTRL + Z
```

Resume:

```bash
fg
```

---

# Daily Cybersecurity Useful Commands

```bash
history

strings binary

file malware

xxd file

lsof

strace

ltrace

ss -tulpn

netstat -antp

find / -perm -4000
```

Useful for:

* enumeration
* malware analysis
* CTF
* privilege escalation

---

# Notes

* `pwd` → current location
* `ls -la` → hidden files
* `grep` → search text
* `find` → search files
* `top` → monitor processes
* `tail -f` → live logs
* `ss` replaces `netstat`
* `CTRL+R` searches command history
* `history` is very useful in investigations

---

*End of Linux Daily Useful Commands Notes*
