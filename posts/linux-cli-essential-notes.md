# Linux Command Line Essentials: Search, Compression, Environment Variables & Redirection Notes

> These commands are heavily used in day-to-day Linux administration, cybersecurity, scripting, CTFs, log analysis, and troubleshooting.

This note covers:

* File comparison
* Compression & extraction
* Sorting and text processing
* `grep`
* Environment variables
* `find`
* Input / Output Redirection

---

# Table of Contents

1. [File Utility Commands](#1-file-utility-commands)
2. [Compression and Archive Commands](#2-compression-and-archive-commands)
3. [Sorting and Text Processing](#3-sorting-and-text-processing)
4. [GREP Notes](#4-grep-notes)
5. [Linux Environment Variables](#5-linux-environment-variables)
6. [Find Command Notes](#6-find-command-notes)
7. [Input Output Redirection](#7-input-output-redirection)
8. [Cybersecurity Use Cases](#8-cybersecurity-use-cases)

---

# 1. File Utility Commands

---

## Count Words, Lines and Characters

```bash
wc <filename>
```

Example:

```bash
wc data.txt
```

Output:

```text
15 120 800 data.txt
```

Meaning:

| Field | Meaning    |
| ----- | ---------- |
| 15    | lines      |
| 120   | words      |
| 800   | characters |

---

## Compare Two Files

```bash
diff file1 file2
```

Example:

```bash
diff old.txt new.txt
```

Shows differences line-by-line.

Useful for:

* comparing configs
* detecting modifications
* change analysis

---

# 2. Compression and Archive Commands

---

## Compress Multiple Files into Archive

```bash
tar -cvf archive.tar file1 file2 file3
```

Options:

| Option | Meaning  |
| ------ | -------- |
| c      | create   |
| v      | verbose  |
| f      | filename |

---

## Compress Using GZIP

```bash
gzip archive.tar
```

Creates:

```text
archive.tar.gz
```

---

## Decompress GZIP File

```bash
gunzip archive.tar.gz
```

---

## Create and Compress Directly

```bash
tar -czvf archive.tar.gz folder
```

Options:

| Option | Meaning          |
| ------ | ---------------- |
| z      | gzip compression |

---

## Extract Compressed Archive

```bash
tar -xzvf archive.tar.gz
```

---

# 3. Sorting and Text Processing

---

## Combine Multiple Files

```bash
cat file1 file2 > file3
```

Combines content:

```text
file1
+
file2
↓
file3
```

---

## Sort File Contents

```bash
sort filename
```

Example:

```bash
sort users.txt
```

Sorts alphabetically.

---

## Sort and Remove Duplicate Entries

```bash
sort filename | uniq
```

Example:

```bash
sort users.txt | uniq
```

Useful for:

* IOC cleanup
* duplicate IP removal
* log processing

---

# 4. GREP Notes

`grep` is used to search text patterns.

General syntax:

```bash
grep [option] pattern filename
```

---

## Basic Search

```bash
grep keyword file.txt
```

---

## Ignore Case

```bash
grep -i keyword file.txt
```

Matches:

```text
Admin
admin
ADMIN
```

---

## Print Everything Except Match

```bash
grep -iv keyword file.txt
```

`-v` means:

```text
invert match
```

---

## Count Occurrences

```bash
grep -c keyword file.txt
```

---

## Match Exact Word

```bash
grep -w keyword file.txt
```

Prevents partial matches.

Example:

Searching:

```text
admin
```

Will NOT match:

```text
administrator
```

---

## Show Matching Line Numbers

```bash
grep -n keyword file.txt
```

---

## Search in Multiple Files

```bash
grep keyword file1 file2 file3
```

---

## Search Multiple Patterns

```bash
grep -e key1 -e key2 file.txt
```

---

## Multiple Patterns Across Multiple Files

```bash
grep -e key1 -e key2 file1 file2
```

---

## Show Matching File Names Only

```bash
grep -l keyword file1 file2
```

---

## Read Patterns from File

Suppose:

```text
keywords.txt
```

contains:

```text
admin
password
secret
```

Use:

```bash
grep -f keywords.txt logfile.txt
```

---

## Match Start of Line

```bash
grep "^keyword" file.txt
```

`^`

means:

```text
Start of line
```

---

## Match End of Line

```bash
grep "keyword$" file.txt
```

`$`

means:

```text
End of line
```

---

## Search Entire Directory Recursively

```bash
grep -R keyword dirA
```

Searches:

```text
all files recursively
```

---

## Use egrep for Multiple Keywords

```bash
egrep "key1|key2|key3" file.txt
```

Equivalent:

```bash
grep -E "key1|key2|key3" file.txt
```

---

## Print Lines After Match

```bash
grep keyword -A 15 file.txt
```

Shows:

```text
15 lines after match
```

Also useful:

```bash
grep keyword -B 10
```

Before match

```bash
grep keyword -C 5
```

Before + After

---

# 5. Linux Environment Variables

Environment variables are:

> Dynamic named values used by programs and shells.

Examples:

```text
PATH
HOME
USER
SHELL
PWD
```

---

## View Variables

```bash
env
```

---

## View Single Variable

```bash
echo $HOME
```

---

## Create Temporary Variable

```bash
export NAME=Jeet
```

Verify:

```bash
echo $NAME
```

---

## Remove Variable

```bash
unset NAME
```

---

## Permanent User Variables

Edit:

```bash
~/.bashrc
```

Add:

```bash
export NAME=Jeet
```

Apply:

```bash
source ~/.bashrc
```

---

## Global Variables

Edit:

```bash
/etc/profile
```

Apply:

```bash
source /etc/profile
```

---

# 6. Find Command Notes

General syntax:

```bash
find <path> [options]
```

---

## Search by Size

```bash
find /home -size 50M
```

Units:

| Symbol | Meaning   |
| ------ | --------- |
| M      | Megabytes |
| K      | Kilobytes |
| G      | Gigabytes |
| c      | bytes     |

---

## Find Only Files

```bash
find . -type f
```

---

## Find Directories

```bash
find . -type d
```

---

## Symbolic Links

```bash
find . -type l
```

---

## Search by Filename

```bash
find . -name file.txt
```

---

## Ignore Case

```bash
find . -iname file.txt
```

---

## Find Files by Owner

```bash
find . -user root
```

---

## Search by Link Count

```bash
find . -links 2
```

---

## Search by Permissions

```bash
find . -perm /u=r
```

---

## Search Files Starting with a

```bash
find . -iname "a*"
```

---

## Find Empty Files

```bash
find . -empty
```

---

## Delete Empty Files

```bash
find . -empty -exec rm {} \;
```

Explanation:

```text
{} → current result
\; → terminate command
```

---

## Search Between 1–50 MB

```bash
find . -size +1M -size -50M
```

---

## Find 15-Day Old Files

```bash
find . -mtime 15
```

---

# 7. Input Output Redirection

Linux has three standard streams:

| Name   | Descriptor |
| ------ | ---------: |
| stdin  |          0 |
| stdout |          1 |
| stderr |          2 |

---

# stdout (1)

Normal command output:

```bash
ls
```

Redirect output:

```bash
ls > file.txt
```

Append:

```bash
ls >> file.txt
```

---

# stderr (2)

Error messages belong here.

Redirect:

```bash
cd /root 2>/errors.txt
```

Discard errors:

```bash
cd /root 2>/dev/null
```

---

## Redirect stdout + stderr

```bash
command >output.txt 2>&1
```

---

## Redirect Everything to Null

```bash
command >/dev/null 2>&1
```

Useful for cron jobs.

---

# stdin (0)

Input stream.

Example:

```bash
cat < file.txt
```

Output:

```text
prints file content
```

---

Normal:

```bash
wc -l file.txt
```

Output:

```text
15 file.txt
```

Using stdin:

```bash
wc -l < file.txt
```

Output:

```text
15
```

No filename shown.

---

# 8. Cybersecurity Use Cases

---

## Find SUID Files

```bash
find / -perm -4000 2>/dev/null
```

---

## Search Password in Configs

```bash
grep -Ri password /etc
```

---

## Search Large Files

```bash
find / -size +100M
```

---

## Remove Duplicate IOCs

```bash
sort ioc.txt | uniq
```

---

## Search Indicators from IOC File

```bash
grep -f ioc.txt logs.txt
```

---

# Notes

* `grep -R` searches recursively
* `sort | uniq` removes duplicates
* `env` lists variables
* `export` creates variable
* `find` is extremely powerful
* `2>/dev/null` suppresses errors
* `stdin=0 stdout=1 stderr=2`
* `grep -A` shows lines after match

---

*End of Linux Command Line Essentials Notes*
