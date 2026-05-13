# Linux AWK Scripting Notes

> AWK is a powerful Linux text-processing and reporting utility mainly used for:
>
> * Field and column extraction
> * Log analysis
> * Filtering data
> * Pattern matching
> * Automation and reporting
> * Mathematical operations on structured text

---

# Table of Contents

1. [How AWK Works](#1-how-awk-works)
2. [AWK Terms Reference](#2-awk-terms-reference)
3. [Basic AWK Commands](#3-basic-awk-commands)
4. [Searching in AWK](#4-searching-in-awk)
5. [AWK with Delimiters](#5-awk-with-delimiters)
6. [Built-in Functions in AWK](#6-built-in-functions-in-awk)
7. [Conditional Statements in AWK](#7-conditional-statements-in-awk)
8. [Loops in AWK](#8-loops-in-awk)
9. [Associative Arrays in AWK](#9-associative-arrays-in-awk)
10. [AWK Variables](#10-awk-variables)
11. [AWK Scripting using BEGIN and END](#11-awk-scripting-using-begin-and-end)
12. [AWK BEGIN/END — Practical Examples](#12-awk-beginend--practical-examples)
13. [Regular Expressions in AWK](#13-regular-expressions-in-awk)
14. [Practical Cybersecurity / Log Analysis Examples](#14-practical-cybersecurity--log-analysis-examples)

---

# 1. How AWK Works

If the data looks like:

```text id="g0l6al"
This is line one
This is line two
```

AWK treats whitespace-separated values as fields:

```text id="frf4o4"
        $1      $2    $3     $4
R1  |  This  |  is  | line | one |
R2  |  This  |  is  | line | two |
```

| Term   | Meaning                 |
| ------ | ----------------------- |
| Row    | Entire line             |
| Field  | Individual column/value |
| Record | Complete line           |

---

# 2. AWK Terms Reference

| Term        | Meaning                         |
| ----------- | ------------------------------- |
| `NR`        | Current line number             |
| `NF`        | Number of fields in current row |
| `$0`        | Entire line                     |
| `$1, $2...` | Particular fields               |
| `FS`        | Input field separator           |
| `OFS`       | Output field separator          |
| `RS`        | Record separator                |
| `ORS`       | Output record separator         |

---

# 3. Basic AWK Commands

## a) Print Specific Column

```bash id="l6m5ng"
awk '{ print $4 }' sample.txt
```

---

## b) Print Multiple Columns

```bash id="xjzbn7"
awk '{ print $1, $3 }' sample.txt
```

---

## c) Print Last Column

```bash id="uvy0gg"
awk '{ print $NF }' sample.txt
```

---

## d) Print Entire Line

```bash id="q5bw6f"
awk '{ print $0 }' sample.txt
```

---

## e) Print with Line Numbers

```bash id="1l7sse"
awk '{ print NR, $0 }' sample.txt
```

---

## f) Print Specific Line

```bash id="mubqz7"
awk 'NR==5 { print $0 }' sample.txt
```

---

## g) Print Range of Lines

```bash id="3f17lj"
awk 'NR>=3 && NR<=6 { print $0 }' sample.txt
```

---

## h) Print Blank Line Numbers

```bash id="6e3v0q"
awk 'NF==0 { print NR }' sample.txt
```

---

## i) Count Total Lines

```bash id="p57kns"
awk 'END { print NR }' sample.txt
```

---

## j) Count Fields Per Line

```bash id="l7n2p4"
awk '{ print NF }' sample.txt
```

---

# 4. Searching in AWK

## a) Search for a Word

```bash id="ux4k1w"
awk '/India/ { print $0 }' sample.txt
```

---

## b) Search Multiple Words

```bash id="t14kq0"
awk '/India/ || /USA/ { print $0 }' sample.txt
```

---

## c) Ignore Case While Searching

```bash id="9q6byh"
awk 'BEGIN { IGNORECASE=1 } /jeet/ { print $0 }' sample.txt
```

---

## d) Search in Particular Column

```bash id="5l72j5"
awk '$2 ~ /India/ { print $0 }' sample.txt
```

---

## e) Search Using NOT Condition

```bash id="1wm9ei"
awk '!/India/ { print $0 }' sample.txt
```

---

# 5. AWK with Delimiters

## a) Use Comma as Delimiter

```bash id="1p76nn"
awk -F, '{ print $2 }' sample.txt
```

---

## b) Use Multiple Delimiters

```bash id="4ff9q6"
awk -F'[,:-]' '{ print $2 }' sample.txt
```

---

## c) Change Output Delimiter

```bash id="epmwlb"
awk 'BEGIN { OFS=" | " } { print $1, $2, $3 }' sample.txt
```

---

## d) Remove Comma from CSV Output

```bash id="5b4m6p"
awk -F, '{ gsub(",", " "); print $0 }' sample.txt
```

---

## e) Remove Double Quotes

```bash id="whqeh0"
awk -F, '{ gsub("\"", " "); print $0 }' sample.txt
```

---

## f) Print Data Between Time Range

```bash id="jr5fmu"
awk '$3 >= "01:05:55" && $3 <= "01:10:00" { print $0 }' log.txt
```

---

# 6. Built-in Functions in AWK

## a) Replace Word Using `gsub()`

```bash id="4dzp0k"
awk '{ gsub("Hello", "World"); print $0 }' sample.txt
```

---

## b) String Length

```bash id="69nlup"
awk '{ print length($2) }' sample.txt
```

---

## c) Find Position of Word

```bash id="ggoj0s"
awk '/paul/ { print index($0, "paul") }' sample.txt
```

---

## d) Convert to Uppercase

```bash id="u8vbh5"
awk '{ print toupper($2) }' sample.txt
```

---

## e) Convert to Lowercase

```bash id="oqotd4"
awk '{ print tolower($2) }' sample.txt
```

---

## f) Split String into Array

```bash id="i0opzm"
awk '{ split($0, arr, " "); print arr[1] }' sample.txt
```

---

## g) Extract Substring

```bash id="s4uvvq"
awk '{ print substr($1,1,3) }' sample.txt
```

---

# 7. Conditional Statements in AWK

## a) Basic IF Condition

```bash id="1n5u7g"
awk '{ if ($NF > 500) print "High"; else print "Low" }' file.txt
```

---

## b) Multiple Conditions

```bash id="qr4l8p"
awk '{ if ($NF > 700) print "High"; else if ($NF > 500) print "Medium"; else print "Low" }' file.txt
```

---

## c) AND Condition

```bash id="wbpkwo"
awk '$2=="India" && $NF > 500 { print $0 }' file.txt
```

---

## d) OR Condition

```bash id="ykjlwm"
awk '$2=="India" || $2=="USA" { print $0 }' file.txt
```

---

# 8. Loops in AWK

## a) FOR Loop

```bash id="o5xh6p"
awk '{ for(i=1;i<=NF;i++) print $i }' sample.txt
```

---

## b) Print All Fields with Index

```bash id="v2tv6n"
awk '{ for(i=1;i<=NF;i++) print i, $i }' sample.txt
```

---

# 9. Associative Arrays in AWK

> Associative arrays are one of the most powerful features of AWK.
>
> Unlike normal arrays:
>
> ```c id="8a0z0m"
> arr[0]
> arr[1]
> ```
>
> AWK arrays can use strings as keys:
>
> ```awk id="knzt4u"
> count["India"]
> count["USA"]
> ```
>
> AWK internally stores associative arrays like key-value mappings.

---

## Internal Concept

When AWK sees:

```awk id="mry8p0"
count["India"]++
```

It internally behaves like:

```text id="i8uhjr"
India -> 1
USA    -> 2
```

---

## a) Basic Associative Array Example

```bash id="hzorlj"
awk '{ count[$1]++ } END { print count["India"] }' file.txt
```

### Example Input

```text id="xbfd37"
India
USA
India
Canada
India
```

### Output

```text id="jxvnp9"
3
```

---

## Step-by-Step Understanding

### Line 1

```text id="y16n6g"
India
```

Execution:

```awk id="y0sk1v"
count["India"]++
```

Now:

```text id="3j17ec"
India -> 1
```

---

### Line 2

```text id="vbh0ud"
USA
```

Now:

```text id="6w4h3v"
India -> 1
USA    -> 1
```

---

### Final State

```text id="1jkh2s"
India  -> 3
USA    -> 1
Canada -> 1
```

---

## b) Count Occurrences of Every Word

```bash id="k7i5yq"
awk '{ count[$1]++ } END { for(word in count) print word, count[word] }' file.txt
```

---

## Understanding `for(i in array)`

```awk id="ij25jp"
for(i in count)
```

means:

> Iterate through every key inside the associative array.

---

## c) Count Requests Per IP Address

```bash id="7n0m5l"
awk '{ ip[$1]++ } END { for(i in ip) print i, ip[i] }' access.log
```

---

## d) Detect Brute Force Attempts

```bash id="7skwx0"
awk '{ failed[$1]++ } END { for(ip in failed) if(failed[ip] > 10) print ip }' auth.log
```

---

## e) Sum Values Grouped by Key

### Example: Total Salary by Department

```bash id="w59h7d"
awk '{ dept[$1] += $2 } END { for(d in dept) print d, dept[d] }' salary.txt
```

### Example Input

```text id="8v5x3l"
IT 500
IT 600
Medical 700
IT 400
```

### Output

```text id="8pqk17"
IT 1500
Medical 700
```

---

## f) Check Whether Key Exists

```bash id="5dbp6d"
awk '{ seen[$1]=1 } END { if("India" in seen) print "Found" }' file.txt
```

> `in` checks whether a key exists.

---

## g) Delete Array Element

```bash id="1jwdgv"
awk '{ arr[$1]=$2 } END { delete arr["Jeet"] }' file.txt
```

---

## Important Concepts

### Arrays Auto-Initialize

If key does not exist:

```awk id="mlb3z9"
count["India"]
```

AWK automatically treats it as:

```awk id="l2o62o"
0
```

So this works immediately:

```awk id="jlwm5g"
count["India"]++
```

No manual initialization needed.

---

## Associative Arrays Can Store Strings

```bash id="it4l0q"
awk 'BEGIN {
    user["jeet"]="admin"
    print user["jeet"]
}'
```

### Output

```text id="3tx8u8"
admin
```

---

## Mental Model

Think of AWK associative arrays like Python dictionaries:

| Python               | AWK                |
| -------------------- | ------------------ |
| `dict["India"] += 1` | `count["India"]++` |
| `for k in dict:`     | `for(i in count)`  |

---

# 10. AWK Variables

## a) User-Defined Variables

```bash id="jzktn0"
awk '{ sum = sum + $NF } END { print sum }' file.txt
```

---

## b) Pass Shell Variable into AWK

```bash id="yp82de"
awk -v name="Jeet" '$1==name { print $0 }' file.txt
```

---

# 11. AWK Scripting using BEGIN and END

## General Structure

```bash id="d0qahw"
awk 'BEGIN { ... } { ... } END { ... }' sample.txt
```

| Block      | Purpose                      |
| ---------- | ---------------------------- |
| `BEGIN`    | Executes before reading file |
| Main Block | Executes for every line      |
| `END`      | Executes after processing    |

---

# 12. AWK BEGIN/END — Practical Examples

## a) Calculate Total Salary

```bash id="3uvl2x"
awk 'BEGIN { sum=0 } NR>1 { sum += $NF } END { print "Total:", sum }' file.txt
```

---

## b) Count Total Employees

```bash id="esujwd"
awk 'NR>1 { count++ } END { print "Employees:", count }' file.txt
```

---

## c) Print Employees with Salary > 500

```bash id="mj6e6c"
awk '$NF > 500 { print $1 }' file.txt
```

---

## d) Calculate Average Salary

```bash id="3ty3nr"
awk 'NR>1 { sum += $NF; count++ } END { print "Average:", sum/count }' file.txt
```

---

# 13. Regular Expressions in AWK

| Symbol | Meaning                  |
| ------ | ------------------------ |
| `^`    | Start of line            |
| `$`    | End of line              |
| `.`    | Any single character     |
| `*`    | Zero or more occurrences |
| `[]`   | Character set            |
| `[^]`  | Negated set              |

---

## a) Match Start of Line

```bash id="4d7qke"
awk '/^Error/' logs.txt
```

---

## b) Match End of Line

```bash id="7kjlwm"
awk '/failed$/' logs.txt
```

---

## c) Match Numeric Lines

```bash id="wluq78"
awk '/^[0-9]+$/' file.txt
```

---

## d) Match IP Address Pattern

```bash id="1s0b41"
awk '/[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+/' access.log
```

---

# 14. Practical Cybersecurity / Log Analysis Examples

## a) Extract Failed SSH Login Attempts

```bash id="5o4k2f"
awk '/Failed password/ { print $0 }' auth.log
```

---

## b) Print Source IPs

```bash id="d3r4ht"
awk '{ print $1 }' access.log
```

---

## c) Count Requests Per IP

```bash id="fq2u8l"
awk '{ count[$1]++ } END { for(ip in count) print ip, count[ip] }' access.log
```

---

## d) Detect Possible Brute Force Attempts

```bash id="8g8efj"
awk '{ count[$1]++ } END { for(ip in count) if(count[ip] > 10) print ip }' auth.log
```

---

## e) Extract HTTP Status Codes

```bash id="ifjlwm"
awk '{ print $9 }' access.log
```

---

## f) Extract URLs Returning 500 Errors

```bash id="5dftxm"
awk '$9==500 { print $7 }' access.log
```

---

## g) Remove Empty IOC Lines

```bash id="l7m6tv"
awk 'NF > 0' iocs.txt
```

---

## h) Find Suspicious PowerShell Commands

```bash id="wbo7i7"
awk '/powershell/ || /EncodedCommand/' windows_logs.txt
```

---

# Notes

* AWK is field-oriented.
* Default delimiter is whitespace.
* `-F` changes input delimiter.
* `NR` = current line number.
* `NF` = number of fields.
* `$NF` = last field.
* `BEGIN` and `END` are special blocks.
* `gsub()` performs global substitution.
* `~` is used for regex matching.
* Associative arrays are key-value based structures.

---

*End of AWK Notes*
