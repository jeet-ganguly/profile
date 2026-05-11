# Linux Administration Notes

> NOTES — covering core Linux administration topics.

---

## Table of Contents

1. [Linux Process Management](#1-linux-process-management)
2. [Linux Process Status Monitoring](#2-linux-process-status-monitoring)
3. [Linux Process Killing / Termination](#3-linux-process-killing--termination)
4. [Linux Types of Files](#4-linux-types-of-files)
5. [Linux Disk Space Monitoring](#5-linux-disk-space-monitoring)
6. [Linux Memory / RAM Usage Monitoring](#6-linux-memory--ram-usage-monitoring)
7. [Linux Service Management](#7-linux-service-management)
8. [Linux Access Control (ACL)](#8-linux-access-control-acl)

---

## 1. Linux Process Management

Here we learn how to manage processes in Linux — like how to make them run in the background, foreground, etc.

### a) Check Active Tasks in an Open Terminal

```bash
jobs
```

### b) Resume a Forcefully Stopped Process

If a process was stopped forcefully and you want to start it in the background or foreground:

```bash
bg    # Resume in background
fg    # Resume in foreground
```

### c) Select a Specific Stopped Process (Multiple Stopped Processes)

```bash
jobs                  # i)  List all jobs and find the Job ID
bg %<number_id>       # ii) Resume specific job in background
fg %<number_id>       # iii) Resume specific job in foreground
```

### d) Process Priority — Nice Value

In Linux, to give more priority to a task you should be **root** and need to set the **"nice value"** between **-20 to 19**.

> The **lower** the number, the **more priority** the task gets.

```bash
nice -n <number> <pid>
# number must be between -20 to 19
```

### e) Keep a Process Running After Closing Terminal

If anyone wants to run a process and keep it running even after the terminal is closed:

```bash
nohup <program-file-path> &
nohup <program-file-path> > /dev/null 2>&1 &
```

---

## 2. Linux Process Status Monitoring

Here, if you want to monitor all running processes in your Linux system.

### a) Check Running Processes in Current Terminal

```bash
ps
```

The output shows:
| Column | Description |
|--------|-------------|
| `PID`  | Process ID |
| `TTY`  | Terminal type the user is logged into |
| `TIME` | Amount of CPU time (min:sec) the process has been running |
| `CMD`  | The name of the command that launched the process |

### b) See All Running Processes

```bash
ps -e
ps -ef
```

### c) See All Running Processes in BSD Format

```bash
ps aux
```

### d) See Running Processes by Username or Group

```bash
ps -u <username>
ps -G <group_name>
```

### e) See the Process Tree

```bash
ps -ejH
```

---

## 3. Linux Process Killing / Termination

Sometimes we need to kill certain processes.

### a) Basic Syntax of Killing a Process

```bash
kill <options> <pid>
```

> `options` = Signal name or signal number

### b) Check All Available Signals and Their Numbers

```bash
kill -l
```

### c) Commonly Used Kill Commands

```bash
kill <pid>         # Default kill (SIGTERM)
kill -1 <pid>      # Restart the process (SIGHUP)
kill -2 <pid>      # Interrupt from keyboard (SIGINT)
kill -9 <pid>      # Forcefully terminate (SIGKILL)
kill -15 <pid>     # Kill the process gracefully (SIGTERM)
```

---

## 4. Linux Types of Files

### File Symbol Reference Table

| File Symbol | File Type |
|:-----------:|-----------|
| `-`         | Regular file |
| `d`         | Directory |
| `l`         | Link file (symlink) |
| `c`         | Device file (Character device) |
| `s`         | Socket file |
| `p`         | FIFO or Named Pipe |
| `b`         | Block device |

---

### a) `s` — Socket File

A special file used to enable **communication between two processes**.

```
Example: /run/chrony/chronyd.sock
```

---

### b) `p` — FIFO or Named Pipe

Sends data from one process to another, so that the receiving process reads the data in **FIFO (First In, First Out)** manner.

```bash
mkfifo    # Command to create a FIFO file
```

---

### c) `b` — Block Device

A file that refers to a device, but the content of the file is stored in a **block-by-block** manner.

```
Example: /dev/sda1
```

---

### d) `c` — Character Device File

Also refers to a device, but the content is stored **character by character**.

```bash
mknod    # Command to create character device files
```

```
Example: /dev/input/mouse2
```

---

### e) `l` — Link File

Used to **link the content of one file with another** using a symlink.

---

## 5. Linux Disk Space Monitoring

### a) Show Space Information About the File System (`df`)

```bash
df -h        # Human readable format
df -BM       # Show size in MB
df -BG       # Show size in GB
df -BK       # Show size in KB
```

---

### b) Show Disk Usage of Files / Folders / Directories (`du`)

```bash
du -h                    # Human readable — shows info about current & recursive folders
du -h <folder-path>      # Show disk usage of a specific folder
du -ah <folder-path>     # Show disk usage of all available files inside the folder
du -ah -BM <folder-path> # In MB
du -ah -BG <folder-path> # In GB
du -ah -BK <folder-path> # In KB
```

> **Note:** By default, `du` shows output in MB.

---

## 6. Linux Memory / RAM Usage Monitoring

### a) Display Free and Used Memory in the System

```bash
free -h          # Human readable format
free -s n        # Keep refreshing memory info every "n" seconds
free -c n        # Exit after repeating the command "n" times (every 1 second)
```

---

## 7. Linux Service Management

In Linux, we create **services for automation**.

### a) Start and Stop a Service

```bash
sudo systemctl start <service_name>
sudo systemctl stop <service_name>
```

### b) Check the Status of a Service

```bash
sudo systemctl status <service_name>
```

### c) Restart a Service

```bash
sudo systemctl restart <service_name>
```

### d) Start a Service Automatically After PC Restart

```bash
sudo systemctl enable <service_name>
```

### e) Start a Service Now AND Enable it on PC Restart

```bash
sudo systemctl enable --now <service_name>
```

### f) Check if a Service is Enabled

```bash
sudo systemctl is-enabled <service_name>
```

### g) Disable a Service After System Reboot

```bash
sudo systemctl disable <service_name>
```

### h) Prevent a Service from Being Started (Mask)

```bash
sudo systemctl mask <service_name>
```

> If a service is **masked**, no one can start that service.

### i) Unmask a Service

```bash
sudo systemctl unmask <service_name>
```

### j) Check All Running Services

```bash
sudo systemctl | grep -i Running
```

---

## 8. Linux Access Control (ACL)

Linux Access Control is basically **adding access permissions to a specific user or group**.

### The Problem with `chmod` / `chown`

Although we have `chmod` and `chown`, the problem is:

> Let's say **Jeet**, **Pritam**, and **Prasanna** are 3 users in Ubuntu, and you have a file with permission:
> ```
> rw-rw-r--
> ```
> If you want to give write permission to **"Pritam"** only — using `chmod` or `chown` would also give the permission to **"Prasanna"**.
>
> If files have ACL, permission will show as:
> ```
> rw-rw-r--+
> ```
> The **`+`** at the end indicates ACL is set. For such situations, we use **ACL**.

---

### x) Check ACL Permissions

```bash
getfacl <file-name>
```

### a) Add Permission for a Specific User

```bash
setfacl -m u:<username>:<r/w/x> <target-file>
```

### b) Add Permission for a Group

```bash
setfacl -m g:<group>:<r/w/x> <target-file>
```

### c) Remove a Specific Permission

```bash
setfacl -x u:<username>:<r/w/x> <target-file>
```

### d) Remove All ACL Entries

```bash
setfacl -b <target-file>
```

### e) Add Permission for a User in All Files Inside a Folder (Recursive)

```bash
setfacl -Rm "entry" <target-folder>
setfacl -Rm u:<username>:<r/w/x> <target-folder>
```

---

*End of Notes*