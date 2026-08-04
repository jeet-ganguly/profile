# Windows PowerShell — Operators & If-Else Statements

> **Operators** are symbols or keywords used to perform operations on variables and values. They allow PowerShell to perform calculations, compare values, assign data, and evaluate logical expressions.
>
> **If-Else statements** are decision-making statements that execute different blocks of code based on whether a condition is **True** or **False**.
>
> Together, operators and conditional statements form the foundation of PowerShell scripting.

These notes cover:

- What are Operators
- Types of Operators
- Arithmetic Operators
- Assignment Operators
- Comparison Operators
- Logical Operators
- Special Operators
- What is If Statement
- If Statement
- If-Else Statement
- If-ElseIf-Else Statement
- Nested If Statement
- Cybersecurity Perspective
- Quick Revision Sheet

---

# Table of Contents

- [1. What are Operators](#1-what-are-operators)
- [2. Types of Operators](#2-types-of-operators)
- [3. Arithmetic Operators](#3-arithmetic-operators)
- [4. Assignment Operators](#4-assignment-operators)
- [5. Comparison Operators](#5-comparison-operators)
- [6. Logical Operators](#6-logical-operators)
- [7. Special Operators](#7-special-operators)
- [8. What is If Statement](#8-what-is-if-statement)
- [9. If Statement](#9-if-statement)
- [10. If-Else Statement](#10-if-else-statement)
- [11. If-ElseIf-Else Statement](#11-if-elseif-else-statement)
- [12. Nested If Statement](#12-nested-if-statement)
- [13. Cybersecurity Perspective](#13-cybersecurity-perspective)
- [14. Quick Revision Sheet](#14-quick-revision-sheet)

---

# 1. What are Operators

Operators are symbols or keywords that perform operations on variables or values.

Example:

```powershell
5 + 3
```

Here:

```text
+

↓

Operator
```

It performs an addition operation.

---

## Basic Concept

```text
Value

↓

Operator

↓

Result
```

---

# 2. Types of Operators

PowerShell provides many operators.

The most commonly used are:

```text
Arithmetic Operators

Assignment Operators

Comparison Operators

Logical Operators

Special Operators
```

---

# 3. Arithmetic Operators

Arithmetic operators perform mathematical calculations.

| Operator | Purpose | Example |
|----------|----------|----------|
| + | Addition | `5 + 2` |
| - | Subtraction | `5 - 2` |
| * | Multiplication | `5 * 2` |
| / | Division | `10 / 2` |
| % | Modulus (Remainder) | `10 % 3` |

---

## Example

```powershell
$a = 20
$b = 5

$a + $b
```

Output

```text
25
```

---

Another example

```powershell
20 % 3
```

Output

```text
2
```

---

# 4. Assignment Operators

Assignment operators assign values to variables.

---

## Simple Assignment

```powershell
$x = 10
```

---

## Compound Assignment

| Operator | Meaning |
|----------|----------|
| += | Add and Assign |
| -= | Subtract and Assign |
| *= | Multiply and Assign |
| /= | Divide and Assign |

---

Example

```powershell
$x = 10

$x += 5
```

Result

```text
15
```

---

Another example

```powershell
$x = 20

$x -= 5
```

Result

```text
15
```

---

# 5. Comparison Operators

Comparison operators compare two values.

The result is always:

```text
True

or

False
```

---

| Operator | Meaning |
|----------|----------|
| -eq | Equal |
| -ne | Not Equal |
| -gt | Greater Than |
| -lt | Less Than |
| -ge | Greater Than or Equal |
| -le | Less Than or Equal |

---

## Examples

```powershell
5 -eq 5
```

Output

```text
True
```

---

```powershell
10 -gt 5
```

Output

```text
True
```

---

```powershell
8 -lt 2
```

Output

```text
False
```

---

# 6. Logical Operators

Logical operators combine multiple conditions.

| Operator | Meaning |
|----------|----------|
| -and | Both Conditions Must Be True |
| -or | Any One Condition Must Be True |
| -not | Negates a Condition |

---

## Example

```powershell
(10 -gt 5) -and (8 -gt 3)
```

Output

```text
True
```

---

Another example

```powershell
(5 -gt 10) -or (3 -lt 5)
```

Output

```text
True
```

---

Example

```powershell
-not ($true)
```

Output

```text
False
```

---

# 7. Special Operators

PowerShell provides several useful operators.

---

## Contains

Checks whether a collection contains a value.

Example

```powershell
1,2,3 -contains 2
```

Output

```text
True
```

---

## Match

Used with Regular Expressions.

Example

```powershell
"PowerShell" -match "Shell"
```

Output

```text
True
```

---

## Like

Performs wildcard matching.

Example

```powershell
"Administrator" -like "Admin*"
```

Output

```text
True
```

---

## In

Checks whether a value exists inside a collection.

Example

```powershell
2 -in 1,2,3
```

Output

```text
True
```

---

# 8. What is If Statement

An **If Statement** allows PowerShell to make decisions.

It evaluates a condition.

If the condition is:

```text
True

↓

Execute Code
```

If the condition is:

```text
False

↓

Skip Code
```

---

## Basic Concept

```text
Condition

↓

True ?

↓

Yes

↓

Execute

↓

No

↓

Skip
```

---

# 9. If Statement

Syntax

```powershell
if (Condition)
{
    Statements
}
```

---

Example

```powershell
$age = 20

if ($age -ge 18)
{
    "Eligible"
}
```

Output

```text
Eligible
```

---

# 10. If-Else Statement

If the condition is false, the **Else** block executes.

Syntax

```powershell
if (Condition)
{
    Statements
}
else
{
    Statements
}
```

---

Example

```powershell
$age = 16

if ($age -ge 18)
{
    "Eligible"
}
else
{
    "Not Eligible"
}
```

Output

```text
Not Eligible
```

---

# 11. If-ElseIf-Else Statement

Used when multiple conditions need to be checked.

Syntax

```powershell
if (Condition1)
{
}
elseif (Condition2)
{
}
else
{
}
```

---

Example

```powershell
$marks = 75

if ($marks -ge 90)
{
    "Excellent"
}
elseif ($marks -ge 60)
{
    "Good"
}
else
{
    "Fail"
}
```

Output

```text
Good
```

---

## Working

```text
Condition 1

↓

False

↓

Condition 2

↓

True

↓

Execute

↓

Skip Remaining Conditions
```

---

# 12. Nested If Statement

A Nested If is an **If Statement inside another If Statement**.

Example

```powershell
$user = "Admin"
$password = "Pass123"

if ($user -eq "Admin")
{
    if ($password -eq "Pass123")
    {
        "Login Successful"
    }
}
```

---

## Working

```text
Outer Condition

↓

True

↓

Inner Condition

↓

True

↓

Execute Code
```

---

# 13. Cybersecurity Perspective

Operators and conditional statements are heavily used in security automation.

---

## Common Uses

```text
Check Running Processes

Verify Services

Analyze Event Logs

Validate User Input

Check File Existence

Identify Suspicious Activity

Automate Incident Response
```

---

## Example Logic

```text
Event ID = 4625 ?

↓

Yes

↓

Failed Login Detected

↓

Generate Alert
```

---

Another example

```text
File Exists ?

↓

Yes

↓

Calculate Hash

↓

Compare Hash

↓

Known Malware ?

↓

Generate Alert
```

---

## Best Practices

```text
Use Meaningful Conditions

Avoid Unnecessary Nesting

Validate User Input

Use Logical Operators Carefully

Test Every Condition
```

---

# 14. Quick Revision Sheet

Operators

```text
Perform Operations
on Values
```

---

Arithmetic Operators

```text
+

-

*

/

%
```

---

Assignment Operators

```text
=

+=

-=

*=

/=
```

---

Comparison Operators

```text
-eq

-ne

-gt

-lt

-ge

-le
```

---

Logical Operators

```text
-and

-or

-not
```

---

Special Operators

```text
-contains

-match

-like

-in
```

---

Decision Statements

```text
if

if-else

if-elseif-else

Nested if
```

---

Decision Flow

```text
Condition

↓

True ?

↓

Execute

↓

Otherwise

↓

Else Block
```

---

Biggest Concept

```text
Operators perform mathematical,
comparison, logical, and special
operations on data.

If-Else statements use the
result of these operations to
make decisions, allowing scripts
to execute different blocks of
code based on conditions.

Together, operators and
conditional statements form the
foundation of PowerShell
automation and scripting.
```

---

*End of PowerShell Operators & If-Else Notes*