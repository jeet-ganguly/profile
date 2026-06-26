# Linux — `cut` and `tr` Commands

> The `cut` and `tr` commands are commonly used for text processing in Linux.
>
> * **`cut`** extracts specific columns, fields, or characters from text.
> * **`tr`** translates, replaces, deletes, or squeezes characters from standard input.
>
> These commands are widely used in shell scripting, log analysis, CTFs, cybersecurity, and Linux administration.


---

# Table of Contents

* [1. Introduction to cut](#1-introduction-to-cut)
* [2. Sample File](#2-sample-file)
* [3. Character Extraction](#3-character-extraction)
* [4. Field Extraction](#4-field-extraction)
* [5. Custom Delimiter](#5-custom-delimiter)
* [6. Common cut Options](#6-common-cut-options)
* [7. Introduction to tr](#7-introduction-to-tr)
* [8. Character Translation](#8-character-translation)
* [9. Case Conversion](#9-case-conversion)
* [10. Character Deletion](#10-character-deletion)
* [11. Character Squeezing](#11-character-squeezing)
* [12. Practical Examples](#12-practical-examples)
* [13. Cybersecurity Perspective](#13-cybersecurity-perspective)
* [14. Quick Revision Sheet](#14-quick-revision-sheet)

---

# 1. Introduction to `cut`

The **cut** command extracts selected portions of text from each line of a file.

It is commonly used to extract:

* Characters
* Columns
* Fields

General Syntax:

```bash
cut [OPTION]... [FILE]
```

---

# 2. Sample File

Suppose `employee.txt` contains:

```text
Name:Country:Department:Salary
Jeet:India:IT:50000
Arpan:India:Medical:65000
Prasanna:USA:IT:70000
Pritam:Canada:HR:60000
```

---

# 3. Character Extraction

## Extract Specific Character Position

Syntax:

```bash
cut -c <position> filename
```

Example:

```bash
cut -c 1 employee.txt
```

Output:

```text
N
J
A
P
P
```

Prints the first character of every line.

---

## Extract Multiple Characters

```bash
cut -c 1-5 employee.txt
```

Prints characters from position **1 to 5**.

---

## Extract Multiple Character Ranges

```bash
cut -c 1-3,8-10 employee.txt
```

Prints:

* Characters 1–3
* Characters 8–10

---

# 4. Field Extraction

The **-f** option extracts fields.

Syntax:

```bash
cut -f <field-number> filename
```

---

## Extract First Field

```bash
cut -d ":" -f1 employee.txt
```

Output:

```text
Name
Jeet
Arpan
Prasanna
Pritam
```

---

## Extract Second Field

```bash
cut -d ":" -f2 employee.txt
```

Output:

```text
Country
India
India
USA
Canada
```

---

## Extract Multiple Fields

```bash
cut -d ":" -f1,3 employee.txt
```

Output:

```text
Name:Department
Jeet:IT
Arpan:Medical
Prasanna:IT
Pritam:HR
```

---

## Extract Field Range

```bash
cut -d ":" -f2-4 employee.txt
```

Prints:

* Country
* Department
* Salary

---

# 5. Custom Delimiter

By default, `cut` uses:

```text
TAB
```

as the delimiter.

To specify another delimiter, use:

```bash
-d
```

Example:

```bash
cut -d ":" -f3 employee.txt
```

Extracts the **Department** field.

---

# 6. Common `cut` Options

| Option               | Description                             |
| -------------------- | --------------------------------------- |
| `-c`                 | Extract characters                      |
| `-f`                 | Extract fields                          |
| `-d`                 | Specify delimiter                       |
| `--complement`       | Print everything except selected fields |
| `--output-delimiter` | Change output delimiter                 |

---

## Example: Exclude First Field

```bash
cut -d ":" -f1 --complement employee.txt
```

Output:

```text
Country:Department:Salary
India:IT:50000
...
```

---

## Change Output Delimiter

```bash
cut -d ":" -f1,2 --output-delimiter=","
```

Output:

```text
Jeet,India
Arpan,India
```

---

# 7. Introduction to `tr`

The **tr (translate)** command replaces, deletes, or squeezes characters.

Unlike `cut`, `tr` works on:

```text
Standard Input (stdin)
```

General Syntax:

```bash
tr [OPTION] SET1 [SET2]
```

---

# 8. Character Translation

## Replace One Character

```bash
echo "hello" | tr 'h' 'H'
```

Output:

```text
Hello
```

---

## Replace Multiple Characters

```bash
echo "abc" | tr 'abc' 'ABC'
```

Output:

```text
ABC
```

---

## Replace Spaces with Underscores

```bash
echo "Hello World" | tr ' ' '_'
```

Output:

```text
Hello_World
```

---

# 9. Case Conversion

## Convert Lowercase to Uppercase

```bash
echo "linux" | tr 'a-z' 'A-Z'
```

Output:

```text
LINUX
```

---

## Convert Uppercase to Lowercase

```bash
echo "LINUX" | tr 'A-Z' 'a-z'
```

Output:

```text
linux
```

---

# 10. Character Deletion

The **-d** option deletes characters.

Syntax:

```bash
tr -d 'characters'
```

---

## Remove Digits

```bash
echo "abc123xyz" | tr -d '0-9'
```

Output:

```text
abcxyz
```

---

## Remove Spaces

```bash
echo "Hello World" | tr -d ' '
```

Output:

```text
HelloWorld
```

---

# 11. Character Squeezing

The **-s** option replaces repeated consecutive characters with a single occurrence.

Syntax:

```bash
tr -s 'character'
```

---

## Remove Multiple Spaces

```bash
echo "Linux     Commands" | tr -s ' '
```

Output:

```text
Linux Commands
```

---

## Remove Repeated Blank Lines

```bash
cat file.txt | tr -s '\n'
```

Consecutive blank lines become a single blank line.

---

# 12. Practical Examples

## Extract Usernames from `/etc/passwd`

```bash
cut -d ":" -f1 /etc/passwd
```

---

## Extract Login Shell

```bash
cut -d ":" -f7 /etc/passwd
```

---

## Convert Log File to Uppercase

```bash
cat log.txt | tr 'a-z' 'A-Z'
```

---

## Remove Numbers from Input

```bash
echo "user1234" | tr -d '0-9'
```

---

## Normalize Multiple Spaces

```bash
cat file.txt | tr -s ' '
```

---

## Replace Colon with Comma

```bash
cat employee.txt | tr ':' ','
```

---

# 13. Cybersecurity Perspective

The `cut` and `tr` commands are frequently used in:

* Log Analysis
* Shell Scripting
* CTF Challenges
* Password File Analysis
* Malware Analysis
* SIEM Data Processing

Common examples:

Extract usernames:

```bash
cut -d ":" -f1 /etc/passwd
```

Extract hashes:

```bash
cut -d ":" -f2 shadow.txt
```

Normalize log formatting:

```bash
tr -s ' '
```

Convert captured data to uppercase:

```bash
tr 'a-z' 'A-Z'
```

---

# 14. Quick Revision Sheet

## `cut`

Purpose:

```text
Extract Characters

Extract Fields

Extract Columns
```

Common Options:

```text
-c

-f

-d

--complement
```

---

## `tr`

Purpose:

```text
Translate Characters

Delete Characters

Squeeze Characters
```

Common Options:

```text
-d

-s
```

---

## Frequently Used Commands

```bash
cut -d ":" -f1 file

cut -c 1-5 file

tr 'a-z' 'A-Z'

tr 'A-Z' 'a-z'

tr -d '0-9'

tr -s ' '
```

---

## Biggest Concept

```text
cut
=
Extract selected characters or fields.

tr
=
Translate, replace, delete, or squeeze
characters from standard input.
```

---

*End of Linux cut and tr Commands Notes*
