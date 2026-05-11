# Linux AWK Scripting Notes

> AWK scripting is very useful for Linux automation, string gathering, and particular pattern gathering.

---

## Table of Contents

1. [How AWK Works](#1-how-awk-works)
2. [AWK Terms Reference](#2-awk-terms-reference)
3. [Basic AWK Commands](#3-basic-awk-commands)
4. [Searching in AWK](#4-searching-in-awk)
5. [AWK with Delimiters](#5-awk-with-delimiters)
6. [Built-in Functions in AWK](#6-built-in-functions-in-awk)
7. [AWK Scripting using BEGIN and END](#7-awk-scripting-using-begin-and-end)
8. [AWK BEGIN/END — Practical Examples](#8-awk-beginend--practical-examples)

---

## 1. How AWK Works

If the data looks like:

```
This is line one
This is line two
```

AWK considers **everything as a field** which is **space-separated**:

```
        $1      $2    $3     $4
R1  |  This  |  is  | line |  one  |
R2  |  This  |  is  | line |  two  |
```

Each word = a **field**, each line = a **row (record)**.

---

## 2. AWK Terms Reference

| Term       | Meaning                              |
|------------|--------------------------------------|
| `NR`       | Number of Row (current line number)  |
| `NF`       | Number of Fields in a row            |
| `$0`       | Print everything (entire line)       |
| `$1, $2…`  | Field number assignment              |

---

## 3. Basic AWK Commands

### a) Print Only a Given Column

```bash
awk '{ print $4 }' sample.txt
# Prints column/field no. 4
```

### b) Print Only the Last Column

```bash
awk '{ print $NF }' sample.txt
# $NF stores the last field value by default
```

### e) Printing with Line Numbers

```bash
awk '{ print NR, $0 }' sample.txt
```

### f) Print Only a Specific Line Number

```bash
awk 'NR==<your-line-no.> { print $0 }' sample.txt
# Replace <your-line-no.> with the actual line number
```

### g) Print a Range of Line Numbers (e.g., Line 3 to 6)

```bash
awk 'NR==3, NR==6 { print $0 }' sample.txt
```

### h) Get Line Numbers of Blank Lines

```bash
awk 'NF==0 { print NR }' sample.txt
# NF==0 means number of fields is 0 (blank line)
```

### l) Count Total Number of Lines in a File

```bash
awk 'END { print NR }' data.txt
# After going through all lines, the END block executes
```

---

## 4. Searching in AWK

### d) Search for a Word

```bash
awk '/your_word/ { print $0 }' sample.txt
# Place your searchable word between / ... /
```

### i) Search for Multiple Words (OR logic)

```bash
awk '/paul/ | /jeet/ | /hell/ { print $0 }' sample.txt
# Example: searching for paul, jeet, or hell
```

### j) Search a Word — Ignore Case

```bash
awk 'BEGIN { IGNORECASE=1 } /jeet/ { print $0 }' sample.txt
# IGNORECASE is a built-in AWK variable
```

### k) Check if a Character is Present in a Particular Column

```bash
# Example: word = 'a', column = 2
awk '$2 ~ /a/ { print $0 }' sample.txt
```

---

## 5. AWK with Delimiters

### m) Use a Non-Space Delimiter (e.g., Comma)

```bash
awk -F, '{ print $2 }' sample.txt
# -F sets the field separator; here it's a comma
```

### n) Working with Multiple Delimiters (e.g., , : -)

```bash
awk -F[,:-] '{ print $2 }' sample.txt
```

### o) Print Data Between a Time Frame

```bash
awk '$3 >= "01:05:55" && $3 <= "01:05:66" { print $0 }' log.txt
```

### d) Remove Delimiter (`,`) and Add Space in a CSV File

```bash
awk -F, '{ gsub(",", " "); print $0 }' sample.txt
```

### e) Remove a Specific Character (e.g., `"`)

```bash
awk -F, '{ gsub("\"", " "); print $0 }' sample.txt
```

---

## 6. Built-in Functions in AWK

### 1) Replace a Word (`gsub`)

```bash
# Replace "Hello" with "World"
awk '{ gsub("Hello", "World") }' sample.txt
```

### 2) Get Length of a Field

```bash
awk '{ print length($2) }' sample.txt
# Prints the length of field 2
```

### 3) Index Position of a Word in a Line

```bash
awk '/paul/ { print index($0, "paul") }' sample.txt
# Finds which line 'paul' appeared in,
# then returns the character position of "paul" in that line
```

### 4) Print a Field in Upper or Lower Case

```bash
awk '{ print toupper($5) }' sample.txt   # Uppercase
awk '{ print tolower($5) }' sample.txt   # Lowercase
# Replace $5 with the column number you need
```

---

## 7. AWK Scripting using BEGIN and END

### AWK Structure

```bash
awk 'BEGIN { ... } { print ... } END { ... }' sample.txt
```

| Block      | When it Runs              | Purpose                                        |
|------------|---------------------------|------------------------------------------------|
| `BEGIN { }`| Before processing any line| Variable declaration, print headers            |
| `{ }`      | During runtime (each line)| if-else logic, print, mathematical operations  |
| `END { }`  | After all lines processed | Print final/summary results                    |

---

## 8. AWK BEGIN/END — Practical Examples

### Sample File: `file1.txt`

```
Emp_name    dept.     Salary
Jeet        IT        500
Pritam      IT        600
Prasanna    IT        600
Kailash     IT        600
Bishnu      IT        600
Arpan       Medical   650
```

---

### a) Get Total Salary Given by the Company

```bash
awk 'BEGIN { sum=0 } { sum = sum + $NF } END { print "Total: " sum }' file1.txt
# Salary is in the last column ($NF)
```

---

### b) Get Total Number of Employees

```bash
awk 'NR>1 { if ($NF > 0) count++ } END { print "Total: " count }' file.txt
# NR>1 skips the header row
# By default, each variable stores 0 initially
```

---

### c) Print "High" or "Low" Based on Salary Threshold (>500)

```bash
awk '{ if ($NF > 500) print "High"; else print "Low" }' file.txt
```

---

### d) Remove Comma Separator and Add Space (CSV file)

```bash
awk -F, '{ gsub(",", " "); print $0 }' sample.txt
```

---

### f) Print Employee Names Whose Salary is More Than 500

```bash
awk '{ if ($NF > 500) print $1 }' file1.txt
```

**If names are comma-separated:**

```bash
awk -F, '{ gsub(",", " "); if ($NF > 500) print $1 }' file1.txt
```

---

*End of AWK Notes*