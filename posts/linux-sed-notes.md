# Linux SED Scripting Notes

> SED (Stream Editor) is a Linux text-processing utility mainly used for:
>
> * Search and replace
> * Text transformation
> * Filtering lines
> * Editing files non-interactively
> * Automating repetitive text modifications

---

# Table of Contents

1. [SED vs AWK — Key Differences](#1-sed-vs-awk--key-differences)
2. [Sample File Used in Examples](#2-sample-file-used-in-examples)
3. [Printing Specific Lines](#3-printing-specific-lines)
4. [Searching in SED](#4-searching-in-sed)
5. [Multiple Expressions](#5-multiple-expressions)
6. [Line Range and Step Patterns](#6-line-range-and-step-patterns)
7. [Reading Expressions from External File](#7-reading-expressions-from-external-file)
8. [Replacing Words (Substitution)](#8-replacing-words-substitution)
9. [Deleting Lines](#9-deleting-lines)
10. [Writing Output to Another File](#10-writing-output-to-another-file)
11. [Adding and Editing Lines](#11-adding-and-editing-lines)
12. [Viewing Hidden Characters](#12-viewing-hidden-characters)
13. [Reading Content from Another File](#13-reading-content-from-another-file)
14. [Stopping Execution Early](#14-stopping-execution-early)
15. [Executing External Commands](#15-executing-external-commands)
16. [Displaying Line Numbers](#16-displaying-line-numbers)
17. [SED Regular Expressions](#17-sed-regular-expressions)
18. [POSIX Character Classes](#18-posix-character-classes)
19. [Practical Cybersecurity / Log Analysis Examples](#19-practical-cybersecurity--log-analysis-examples)

---

# 1. SED vs AWK — Key Differences

| Feature              | SED                              | AWK                           |
| -------------------- | -------------------------------- | ----------------------------- |
| Primary Use          | Text transformation and editing  | Data extraction and reporting |
| Operates On          | Characters and lines             | Fields and columns            |
| Programming Features | Limited                          | Full programming features     |
| Best For             | Quick replacements and filtering | Complex parsing and reporting |
| Speed                | Usually faster for simple edits  | Better for structured data    |

---

# 2. Sample File Used in Examples

```text
Name      Country    Designation    Salary
Jeet      India      IT             500
Arpan     India      Medical        650
Prasanna  USA        IT             650
Pritam    Canada     IT             750
Kailash   Nigeria    IT             700
```

---

# 3. Printing Specific Lines

## a) Print a Specific Line

```bash
sed -n '1p' file_name
```

> Prints only line 1.

---

## b) Print a Range of Lines

```bash
sed -n '1,5p' file_name
```

> Prints lines 1 to 5.

---

## c) Print the Last Line

```bash
sed -n '$p' file_name
```

> `$` represents the last line.

---

## d) Print Every 2nd Line Starting from Line 1

```bash
sed -n '1~2p' file_name
```

> Prints lines:
>
> * 1
> * 3
> * 5
> * 7 ...

---

## e) Print Next 4 Lines from Line 2

```bash
sed -n '2,+4p' file_name
```

> Prints line 2 and the next 4 lines.

---

# 4. Searching in SED

## a) Search for a Specific Pattern

```bash
sed -n '/India/p' file_name
```

> Prints all lines containing `India`.

---

## b) Search Multiple Patterns

```bash
sed -n -e '/India/p' -e '/USA/p' file_name
```

> Prints lines containing either `India` or `USA`.

---

# 5. Multiple Expressions

## a) Use Multiple Expressions with `-e`

### Print Multiple Lines

```bash
sed -n -e '2p' -e '5p' file_name
```

### Print Multiple Patterns

```bash
sed -n -e '/India/p' -e '/Germany/p' file_name
```

---

# 6. Line Range and Step Patterns

## a) Print Line Ranges

```bash
sed -n '3,7p' file_name
```

> Prints lines 3 to 7.

---

## b) Print Alternate Lines

```bash
sed -n '1~2p' file_name
```

---

# 7. Reading Expressions from External File

## a) Execute SED Commands from a File

```bash
sed -f commands.sed file_name
```

> `-f` loads SED expressions from another file.

---

## Example

### `commands.sed`

```bash
s/India/USA/g
s/IT/Developer/g
```

### Command

```bash
sed -f commands.sed employees.txt
```

---

# 8. Replacing Words (Substitution)

## a) Replace a Word Globally

```bash
sed 's/old/new/g' file_name
```

> `s` = substitute
> `g` = global replacement

---

## b) Replace Word Only on Specific Line

```bash
sed '5 s/old/new/g' file_name
```

> Changes only line 5.

---

## c) Replace Word on All Lines Except One

```bash
sed '5! s/old/new/g' file_name
```

> `!` means except.

---

## d) Replace and Save Changes Permanently

```bash
sed -i 's/old/new/g' file_name
```

> `-i` edits the file directly.

---

## e) Replace Tab with Space

```bash
sed 's/\t/ /g' file_name
```

---

## f) Change Specific User Information

### Change Salary

```bash
sed '/Arpan/ s/850/650/g' file_name
```

### Change Country

```bash
sed '/Arpan/ s/India/USA/g' file_name
```

> First match the line containing `Arpan`, then apply substitution.

---

# 9. Deleting Lines

## a) Delete First Line

```bash
sed '1d' file_name
```

---

## b) Delete Range of Lines

```bash
sed '1,2d' file_name
```

---

## c) Delete Last Line

```bash
sed '$d' file_name
```

---

## d) Delete Matching Pattern

```bash
sed '/India/d' file_name
```

> Deletes all lines containing `India`.

---

## e) Delete Empty Lines

```bash
sed '/^$/d' file_name
```

> `^$` represents an empty line.

---

# 10. Writing Output to Another File

## a) Save Matching Output into New File

```bash
sed -n '/India/ w india_users.txt' employees.txt
```

> `w` writes matching output to another file.

---

# 11. Adding and Editing Lines

## a) Append Text After Specific Line Number

```bash
sed '5 a new_text' file_name
```

> `a` means append after line.

---

## b) Append Text After Matching Pattern

```bash
sed '/Paul/ a new_text' file_name
```

---

## c) Insert Text Before Matching Pattern

```bash
sed '/Paul/ i new_text' file_name
```

> `i` means insert before line.

---

## d) Replace Entire Line

```bash
sed '5 c new_text' file_name
```

> `c` means change the whole line.

---

# 12. Viewing Hidden Characters

## a) Show Hidden Characters

```bash
sed -n 'l' file_name
```

> Useful for:
>
> * Tabs
> * Hidden spaces
> * Windows line endings

---

## b) Wrap Long Lines

```bash
sed -n 'l 50' file_name
```

> Wraps lines after 50 characters.

---

# 13. Reading Content from Another File

## a) Insert External File Content

```bash
sed '3 r externalfile' file_name
```

> Inserts content of `externalfile` after line 3.

---

# 14. Stopping Execution Early

## a) Quit After First Match

```bash
sed '/India/ q' file_name
```

---

## b) Quit at Specific Line

```bash
sed '5 q' file_name
```

---

## c) Quit with Custom Exit Status

```bash
sed '/India/ q 100' file_name
```

> Useful in shell scripting and automation.

---

# 15. Executing External Commands

## a) Execute Linux Command from SED

```bash
sed '2 e date' file_name
```

> Executes `date` command at line 2.

---

# 16. Displaying Line Numbers

## a) Print Line Numbers

```bash
sed '=' file_name
```

---

# 17. SED Regular Expressions

| Symbol | Meaning                  |
| ------ | ------------------------ |
| `^`    | Start of line            |
| `$`    | End of line              |
| `.`    | Any single character     |
| `[]`   | Character set            |
| `[^]`  | Negated set              |
| `*`    | Zero or more occurrences |

---

## a) Match Line Starting with `2`

```bash
sed -n '/^2/p' file_name
```

---

## b) Match Line Ending with `ia`

```bash
sed -n '/ia$/p' file_name
```

---

## c) Find 5-Letter Name Starting with `S` and Ending with `a`

```bash
sed -n '/^S...a$/p' names
```

### Breakdown

| Pattern | Meaning          |
| ------- | ---------------- |
| `^S`    | Starts with S    |
| `...`   | Any 3 characters |
| `a$`    | Ends with a      |

---

## d) Match Names Starting with `V`

```bash
sed -n '/^V/p' names
```

---

## e) Match Names Ending with `a`

```bash
sed -n '/a$/p' names
```

---

## f) Match Names Starting with `A` or `C`

```bash
sed -n '/^[AC]/p' names
```

---

## g) Match Names Starting from `A` to `D`

```bash
sed -n '/^[A-D]/p' names
```

---

# 18. POSIX Character Classes

POSIX classes provide portable character matching.

---

## Example

```bash
sed -n '/[[:digit:]]/p' file_name
```

---

## Common POSIX Classes

| Class       | Meaning               |
| ----------- | --------------------- |
| `[:alnum:]` | Letters and digits    |
| `[:alpha:]` | Alphabetic characters |
| `[:digit:]` | Numbers               |
| `[:blank:]` | Tabs and spaces       |
| `[:lower:]` | Lowercase letters     |
| `[:upper:]` | Uppercase letters     |
| `[:punct:]` | Punctuation           |
| `[:space:]` | Whitespace            |

---

# 19. Practical Cybersecurity / Log Analysis Examples

## a) Extract Failed SSH Logins

```bash
sed -n '/Failed password/p' auth.log
```

---

## b) Remove Empty IOC Lines

```bash
sed '/^$/d' iocs.txt
```

---

## c) Redact Internal IP Addresses

```bash
sed 's/192\.168\.[0-9]*\.[0-9]*/REDACTED/g' logs.txt
```

---

## d) Extract HTTP 500 Errors

```bash
sed -n '/500/p' access.log
```

---

## e) Remove Commented Config Lines

```bash
sed '/^#/d' config.conf
```

---

# Notes

* `-n` suppresses automatic printing.
* `p` prints matched lines.
* `d` deletes lines.
* `s` performs substitution.
* `g` applies replacement globally.
* `-i` edits files permanently.
* Always test commands without `-i` before modifying important files.

---

*End of SED Notes*
