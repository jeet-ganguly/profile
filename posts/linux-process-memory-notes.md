# Linux Process Memory Maps & File Descriptors Notes

> Linux internally manages process memory and I/O resources using **virtual memory mapping** and **file descriptors (FDs)**. Understanding these concepts is useful in:
>
> * Reverse Engineering
> * Malware Analysis
> * Linux Internals
> * Exploit Development
> * Debugging
> * Digital Forensics

---

# Table of Contents

1. [Linux Process Memory](#1-linux-process-memory)
2. [Standard Process Memory Layout](#2-standard-process-memory-layout)
3. [Reading Process Memory Maps](#3-reading-process-memory-maps)
4. [Useful Memory Commands](#4-useful-memory-commands)
5. [Linux File Descriptors](#5-linux-file-descriptors)
6. [Characteristics of File Descriptors](#6-characteristics-of-file-descriptors)
7. [Standard File Descriptors](#7-standard-file-descriptors)
8. [Viewing File Descriptors](#8-viewing-file-descriptors)
9. [Practical Security Examples](#9-practical-security-examples)

---

# 1. Linux Process Memory

Linux gives each process its own:

```text
Virtual Address Space
```

This virtual address space contains several memory regions allocated to a running program.

Each process sees:

```text
Independent Memory Layout
```

even though physical memory may be shared.

---

# 2. Standard Process Memory Layout

Typical Linux process layout:

```text
High Address
     ↓

+----------------+
|     Stack      |
+----------------+

| Shared Library |
+----------------+

|      Heap      |
+----------------+

|      BSS       |
+----------------+

| Initialized    |
|      Data      |
+----------------+

|   Text(Code)   |
+----------------+

Low Address
```

---

## a) Text Segment (Code)

Stores:

* executable machine instructions
* compiled program code

Characteristics:

* usually read-only
* shared between processes

Example:

```c
int main(){
printf("hello");
}
```

Compiled instructions reside here.

---

## b) Data Segment (Initialized Data)

Stores:

* global variables
* static variables

that already contain values.

Example:

```c
int x=100;
```

---

## c) BSS Segment (Uninitialized Data)

Stores:

* global variables
* static variables

initialized as:

```text
0
```

or without explicit value.

Example:

```c
int x;
static int y;
```

Kernel initializes:

```text
0
```

---

## d) Heap

Dynamic memory area.

Grows:

```text
upward ↑
```

Memory allocated through:

```c
malloc()
calloc()
realloc()
```

Example:

```c
char *ptr=malloc(100);
```

Used for:

* dynamic structures
* linked lists
* buffers

---

## e) Stack

Stores:

* local variables
* function calls
* return addresses
* function parameters

Growth direction:

```text
downward ↓
```

Example:

```c
void test(){
int x=5;
}
```

Variable:

```text
x
```

stored on stack.

---

## f) Shared Libraries

Libraries are mapped between:

```text
Heap
and
Stack
```

Examples:

```text
libc
libm
libpthread
```

Loaded only when needed.

---

# 3. Reading Process Memory Maps

Linux provides process memory information through:

```bash
/proc/<PID>/maps
```

This shows:

* memory regions
* addresses
* permissions
* mapped files
* libraries

---

## View Memory Map

```bash
cat /proc/<pid>/maps
```

Example:

```bash
cat /proc/2222/maps
```

Sample output:

```text
00400000-0040b000 r-xp /usr/bin/bash
0060a000-0060b000 rw-p
7fff4c1b8000 stack
```

---

## Understanding Fields

```text
address      permissions    file
```

Example:

```text
00400000-0040b000 r-xp
```

Meaning:

| Part | Meaning         |
| ---- | --------------- |
| r    | Read            |
| w    | Write           |
| x    | Execute         |
| p    | Private mapping |

---

Common permissions:

| Permission | Meaning |
| ---------- | ------- |
| r          | Read    |
| w          | Write   |
| x          | Execute |
| p          | Private |
| s          | Shared  |

---

# View Raw Process Memory

```bash
cat /proc/<pid>/mem
```

Example:

```bash
cat /proc/2222/mem
```

Contains:

```text
Raw process memory
```

Usually requires:

```bash
root
```

or debugging privileges.

---

# 4. Useful Memory Commands

---

## View Memory Statistics

```bash
cat /proc/<pid>/status
```

---

## View Memory Consumption

```bash
pmap <pid>
```

Example:

```bash
pmap 2222
```

---

## Detailed Memory Report

```bash
pmap -x <pid>
```

---

## View Process Memory via top

```bash
top
```

or:

```bash
htop
```

---

# Useful Memory Terms

| Term | Meaning        |
| ---- | -------------- |
| VIRT | Virtual memory |
| RES  | Physical RAM   |
| SHR  | Shared memory  |

---

# 5. Linux File Descriptors

A File Descriptor (FD) is:

```text
Non-negative integer
```

used as an index/handle for opened resources.

Linux treats almost everything as a file.

FDs may represent:

* regular files
* directories
* sockets
* pipes
* devices
* terminals

---

# Why File Descriptors Matter

Processes interact with resources using FDs.

Examples:

```text
Open file
Network socket
Pipe communication
Terminal I/O
```

All use descriptors internally.

---

# 6. Characteristics of File Descriptors

---

## a) Per Process Scope

Each process has:

```text
own FD table
```

managed by kernel.

---

## b) Sequential Allocation

Kernel assigns:

```text
lowest available integer
```

when new resource opens.

Example:

Current:

```text
0
1
2
```

Open new file:

```text
3
```

Open another:

```text
4
```

---

## c) Automatically Managed

Descriptors are created by:

```c
open()
socket()
pipe()
dup()
accept()
```

and released by:

```c
close()
```

---

# 7. Standard File Descriptors

By convention every process begins with:

| FD |   Name | Purpose         |
| -- | -----: | --------------- |
| 0  |  stdin | Standard Input  |
| 1  | stdout | Standard Output |
| 2  | stderr | Standard Error  |

---

Example:

```bash
echo hello
```

Output:

```text
stdout → FD 1
```

Redirect error:

```bash
2>error.txt
```

Redirect output:

```bash
1>output.txt
```

Redirect both:

```bash
command >out.txt 2>&1
```

---

# 8. Viewing File Descriptors

---

## List Open File Descriptors

```bash
ls -l /proc/<pid>/fd
```

Example:

```bash
ls -l /proc/2222/fd
```

Output:

```text
0 -> /dev/pts/0
1 -> /dev/pts/0
2 -> /dev/pts/0
3 -> socket:[2321]
```

---

Explanation:

| FD | Resource       |
| -- | -------------- |
| 0  | stdin          |
| 1  | stdout         |
| 2  | stderr         |
| 3  | network socket |

---

## Human Readable Format

```bash
lsof -p <pid>
```

Example:

```bash
lsof -p 2222
```

Shows:

* opened files
* sockets
* libraries
* descriptors

---

## Check Open Files for User

```bash
lsof -u <username>
```

Example:

```bash
lsof -u jeet
```

---

## List Processes Using Port

```bash
lsof -i :80
```

---

## View Network Connections

```bash
lsof -i
```

---

# 9. Practical Security Examples

---

## a) Malware Analysis

View suspicious process memory:

```bash
cat /proc/<pid>/maps
```

Look for:

* injected libraries
* anonymous mappings
* suspicious regions

---

## b) Detect Reverse Shell Connections

```bash
lsof -i
```

or:

```bash
netstat -antp
```

---

## c) Find Which Program Uses Port 4444

```bash
lsof -i :4444
```

---

## d) Inspect Running Malware File Handles

```bash
ls -l /proc/<pid>/fd
```

Can reveal:

* hidden files
* deleted executables
* sockets

---

# Notes

* Every process gets its own virtual memory.
* Heap grows upward.
* Stack grows downward.
* `/proc/<pid>/maps` shows memory mappings.
* `/proc/<pid>/mem` contains raw process memory.
* File descriptors are integers.
* Linux treats many resources as files.
* `0,1,2` are stdin, stdout, stderr.
* `lsof` is one of the most useful forensic commands.

---

*End of Linux Process Memory Maps & File Descriptor Notes*
