# Linux System Call Tracing Notes (`strace`, `ltrace`, `ptrace`)

> Linux programs communicate with the kernel through **system calls**. Tracing these calls helps understand what a program is doing internally.

Useful for:

* Malware Analysis
* Reverse Engineering
* Debugging
* Digital Forensics
* Exploit Development
* Performance Analysis

---

# Table of Contents

1. [What are System Calls?](#1-what-are-system-calls)
2. [Program Execution Flow](#2-program-execution-flow)
3. [Using strace](#3-using-strace)
4. [Important strace Options](#4-important-strace-options)
5. [Using ltrace](#5-using-ltrace)
6. [strace vs ltrace](#6-strace-vs-ltrace)
7. [Understanding ptrace](#7-understanding-ptrace)
8. [Important ptrace Functions](#8-important-ptrace-functions)
9. [Simple ptrace C Examples](#9-simple-ptrace-c-examples)
10. [Practical Security Examples](#10-practical-security-examples)
11. [Quick Commands Summary](#11-quick-commands-summary)

---

# 1. What are System Calls?

System calls act as an interface between:

```text
User Space
     ↓
Kernel Space
```

Programs cannot directly interact with hardware.

Instead they request services through system calls.

Examples:

```text
open()
read()
write()
fork()
execve()
socket()
connect()
mmap()
clone()
```

---

# 2. Program Execution Flow

```text
Application
     ↓
Library Functions
     ↓
System Calls
     ↓
Linux Kernel
     ↓
Hardware
```

Example:

```c
printf("hello");
```

Internally:

```text
printf()
    ↓
write()
```

---

# Common System Calls

| Call      | Purpose               |
| --------- | --------------------- |
| open()    | Open file             |
| read()    | Read file             |
| write()   | Write data            |
| execve()  | Execute binary        |
| fork()    | Create process        |
| socket()  | Create network socket |
| connect() | Connect network       |
| mmap()    | Map memory            |

---

# 3. Using strace

`strace` traces:

```text
System calls + signals
```

Useful for:

* understanding behavior
* malware analysis
* debugging
* file access monitoring
* network activity analysis

---

## Basic Usage

```bash
strace ls
```

Example output:

```text
execve("/usr/bin/ls")
openat(...)
read(...)
write(...)
```

---

## Attach to Running Process

```bash
strace -p <PID>
```

Example:

```bash
strace -p 2222
```

Stop:

```text
CTRL+C
```

---

## Save Output

```bash
strace -o output.txt program
```

Example:

```bash
strace -o trace.txt firefox
```

---

## Follow Child Processes

```bash
strace -f program
```

Useful when:

```text
fork()
clone()
```

create child processes.

---

## Trace Only File Activity

```bash
strace -e trace=file program
```

---

## Trace Only Network Calls

```bash
strace -e trace=network program
```

---

## Trace Process Calls

```bash
strace -e trace=process program
```

---

# 4. Important strace Options

| Command        | Purpose         |
| -------------- | --------------- |
| strace program | Start tracing   |
| strace -p PID  | Attach process  |
| strace -f      | Child processes |
| strace -c      | Statistics      |
| strace -tt     | Timestamp       |
| strace -o      | Save output     |
| strace -e      | Filter          |

---

## System Call Statistics

```bash
strace -c ls
```

Shows:

```text
%time
calls
errors
```

---

## Add Timestamp

```bash
strace -tt firefox
```

Output:

```text
13:10:11 read(...)
13:10:12 write(...)
```

---

# 5. Using ltrace

`ltrace` traces:

```text
Library function calls
```

Examples:

```text
printf()
malloc()
strcmp()
fopen()
```

---

## Basic Usage

```bash
ltrace ls
```

Example output:

```text
malloc()
printf()
strcmp()
```

---

## Attach Existing Process

```bash
ltrace -p <PID>
```

---

## Save Output

```bash
ltrace -o output.txt program
```

---

## Follow Child Processes

```bash
ltrace -f program
```

---

# Example Difference

Program:

```c
printf("hello");
```

ltrace shows:

```text
printf()
malloc()
```

strace shows:

```text
write()
```

---

# 6. strace vs ltrace

| Feature |             strace |            ltrace |
| ------- | -----------------: | ----------------: |
| Traces  |       System calls |     Library calls |
| Shows   |      read(),open() | printf(),malloc() |
| Focus   | Kernel interaction |     Program logic |

---

# 7. Understanding ptrace

`ptrace()` is a Linux system call used to:

```text
Control another process
```

Many tools internally use:

* strace
* gdb
* ltrace
* radare2
* debuggers

---

Basic structure:

```text
Tracer Process
      ↓
    ptrace()
      ↓
Target Process
```

---

General syntax:

```c
long ptrace(enum request,
            pid_t pid,
            void *addr,
            void *data);
```

---

Parameters:

| Parameter | Purpose        |
| --------- | -------------- |
| request   | Operation type |
| pid       | Target PID     |
| addr      | Memory address |
| data      | Extra data     |

---

# Why ptrace Is Used

Used for:

* process debugging
* reading memory
* modifying memory
* reading registers
* changing execution flow
* tracing system calls

---

# 8. Important ptrace Functions

---

## a) PTRACE_TRACEME

```c
ptrace(PTRACE_TRACEME,0,NULL,NULL);
```

Purpose:

Child tells kernel:

```text
"My parent will trace me"
```

---

## b) PTRACE_ATTACH

```c
ptrace(PTRACE_ATTACH,pid,NULL,NULL);
```

Attach to running process.

---

## c) PTRACE_DETACH

```c
ptrace(PTRACE_DETACH,pid,NULL,NULL);
```

Detach process.

---

## d) PTRACE_PEEKDATA

```c
ptrace(PTRACE_PEEKDATA,pid,address,NULL);
```

Read memory.

Memory trick:

```text
PEEK = Read
```

---

## e) PTRACE_POKEDATA

```c
ptrace(PTRACE_POKEDATA,pid,address,data);
```

Modify memory.

Memory trick:

```text
POKE = Write
```

---

## f) PTRACE_GETREGS

```c
ptrace(PTRACE_GETREGS,pid,NULL,&regs);
```

Read registers.

Examples:

```text
RIP
RSP
RAX
RBX
```

---

## g) PTRACE_SETREGS

```c
ptrace(PTRACE_SETREGS,pid,NULL,&regs);
```

Modify register values.

---

## h) PTRACE_CONT

```c
ptrace(PTRACE_CONT,pid,NULL,NULL);
```

Resume process.

---

## i) PTRACE_SINGLESTEP

```c
ptrace(PTRACE_SINGLESTEP,pid,NULL,NULL);
```

Execute:

```text
One instruction at a time
```

Used in:

* GDB
* debuggers
* reverse engineering

---

# 9. Simple ptrace C Examples

## Example 1: Child Process Allows Tracing

```c
#include<stdio.h>
#include<sys/ptrace.h>
#include<unistd.h>

int main(){

ptrace(PTRACE_TRACEME,0,NULL,NULL);

printf("Tracing enabled\n");

return 0;
}
```

Compile:

```bash
gcc test.c -o test
```

---

Purpose:

```text
Child allows debugger or parent to trace itself
```

---

## Example 2: Attach to Existing Process

```c
#include<stdio.h>
#include<sys/ptrace.h>
#include<stdlib.h>

int main(int argc,char *argv[]){

int pid=atoi(argv[1]);

ptrace(PTRACE_ATTACH,pid,NULL,NULL);

printf("Attached to PID=%d\n",pid);

return 0;
}
```

Run:

```bash
./a.out 1234
```

---

Purpose:

Attach to running process.

Equivalent idea:

```bash
strace -p 1234
```

---

## Example 3: Read Registers

```c
#include<stdio.h>
#include<sys/ptrace.h>
#include<sys/user.h>

int main(){

struct user_regs_struct regs;

/* example */

ptrace(PTRACE_GETREGS,
       1234,
       NULL,
       &regs);

printf("RIP=%llx\n",regs.rip);

}
```

Purpose:

Read instruction pointer.

Useful in:

* exploit debugging
* reverse engineering

---

# 10. Practical Security Examples

## Malware Analysis

```bash
strace malware
```

Observe:

```text
socket()
connect()
open()
execve()
```

---

## Detect File Dropping

```bash
strace -e trace=file malware
```

---

## Monitor Network Activity

```bash
strace -e trace=network suspicious_binary
```

---

## Observe Child Processes

```bash
strace -f suspicious_binary
```

---

## Reverse Shell Detection

```bash
strace nc
```

Look for:

```text
socket()
connect()
accept()
```

---

# 11. Quick Commands Summary

```bash
strace ls

strace -p PID

strace -f binary

strace -c binary

strace -e trace=file binary

strace -e trace=network binary

ltrace binary

ltrace -p PID

ltrace -f binary
```

---

# Notes

* `strace` → system calls
* `ltrace` → library calls
* `ptrace` controls another process
* `strace` internally uses ptrace
* `PEEK` means read memory
* `POKE` means modify memory
* `SINGLESTEP` executes one instruction
* GDB heavily relies on ptrace

---

*End of Linux System Call Tracing Notes*
