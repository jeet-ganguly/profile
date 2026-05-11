# Linux SED Scripting Notes

> SED (Stream Editor) is used for text transformation and editing — primarily search and replace operations on characters and lines.

---

## Table of Contents

1. [SED vs AWK — Key Differences](#1-sed-vs-awk--key-differences)
2. [Sample File Used in Examples](#2-sample-file-used-in-examples)
3. [Printing Specific Lines](#3-printing-specific-lines)
4. [Searching in SED](#4-searching-in-sed)
5. [Multiple Expressions](#5-multiple-expressions)
6. [Line Range and Step Patterns](#6-line-range-and-step-patterns)
7. [Replacing Words (Substitution)](#7-replacing-words-substitution)
8. [Practical Examples](#8-practical-examples)

---

## 1. SED vs AWK — Key Differences

| Feature | SED | AWK |
|---------|-----|-----|
| **Primary Use** | Text transformation and editing (search/replace) | Data extraction, field manipulation, reporting |
| **Operates On** | Characters and lines | Treats a file like a database with columns/fields |
| **Programming Features** | Not full programming features available | Full programming features — variables, loops, arrays, math |
| **Best For** | Generally faster for simple substitution tasks | Highly efficient for complex logic and large structured datasets |

---

## 2. Sample File Used in Examples

```
Name      Country    Designation    Salary
Jeet      India      IT             500
Arpan     India      Medical        650
Prasanna  USA        IT             650
Pritam    Canada     IT             750
Kailash   Nigeria    IT             700
```

---

## 3. Printing Specific Lines

### a) Show Only a Given Line or Range of Lines

```bash
sed -n '1p' file_name          # Prints line 1 only
sed -n '1,5p' file_name        # Prints lines 1 to 5
sed -n '$p' file_name          # Prints the last line
```

> `-n` suppresses default output; `p` explicitly prints the matched line.

---

## 4. Searching in SED

### b) Filter Lines Matching a Specific String (e.g., Country)

```bash
sed -n '/India/p' file_name
# Prints all lines containing "India"
# Replace "India" with any desired string
```

---

## 5. Multiple Expressions

### c) Check Multiple Expressions at Once

```bash
# By line number
sed -n -e '2p' -e '5p' file_name

# By pattern/string
sed -n -e '/India/p' -e '/USA/p' file_name
```

---

## 6. Line Range and Step Patterns

### d) See Next 4 Lines After Line 2

```bash
sed -n '2, +4p' file_name
# Prints line 2 and the next 4 lines (lines 2–6)
```

### e) Print Every 2nd Line Starting from Line 1

```bash
sed -n '1~2p' file_name
# Prints lines 1, 3, 5, 7 ... (every 2nd line after 1st)
```

---

## 7. Replacing Words (Substitution)

### f) Replace a Word Globally

```bash
sed 's/<string-to-change>/<new-string>/g' file_name
```

### g) Replace a Word Only on a Specific Line

```bash
sed '5 s/<string-to-change>/<new-string>/g' file_name
# Changes only at line 5
```

### h) Replace a Word on All Lines EXCEPT a Specific Line

```bash
sed '5! s/<string-to-change>/<new-string>/g' file_name
# ! means "except" — changes everywhere except line 5
```

### i) Replace Word and Save Changes Permanently to File

```bash
sed -i 's/<string-to-change>/<new-string>/g' file_name
# -i flag edits the file in-place (permanently)
```

---

## 8. Practical Examples

### j) Change the Salary and Country of a Specific User (e.g., 'Arpan')

```bash
# Change salary from 850 to 650 for Arpan
sed '/Arpan/ s/850/650/g' file_name

# Change country from India to USA for Arpan
sed '/Arpan/ s/India/USA/g' file_name
```

> **Pattern:** `sed '/match-pattern/ s/old/new/g' file_name`
> → First find the line containing the pattern, then apply substitution on that line only.

---

*End of SED Notes*