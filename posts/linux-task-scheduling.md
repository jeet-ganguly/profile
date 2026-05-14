# Linux Cron Job / Task Scheduling Notes

> Cron is a Linux task scheduler used to automate commands and scripts at specific times or intervals.

---

# Table of Contents

1. [What is a Cron Job?](#1-what-is-a-cron-job)
2. [Why Use Cron Jobs?](#2-why-use-cron-jobs)
3. [How Cron Works](#3-how-cron-works)
4. [Basic Cron Commands](#4-basic-cron-commands)
5. [Cron Table (Crontab) Format](#5-cron-table-crontab-format)
6. [Cron Scheduling Examples](#6-cron-scheduling-examples)
7. [Special Cron Strings](#7-special-cron-strings)
8. [Redirecting Cron Output](#8-redirecting-cron-output)
9. [Environment Variables in Cron](#9-environment-variables-in-cron)
10. [Cron Logs](#10-cron-logs)
11. [Task Scheduling using `at` Command](#11-task-scheduling-using-at-command)
12. [Practical Cybersecurity / Automation Examples](#12-practical-cybersecurity--automation-examples)

---

# 1. What is a Cron Job?

> A cron job is a scheduled task that automatically runs commands or scripts at specified intervals.

Cron jobs are commonly used for:

* backups,
* automation,
* monitoring,
* maintenance,
* log cleanup,
* scheduled scripting.

---

# 2. Why Use Cron Jobs?

Cron jobs automate repetitive tasks without manual intervention.

---

## Common Uses

* Backup files
* Run maintenance scripts
* Execute monitoring tools
* Rotate logs
* Perform periodic scans
* Send automated reports

---

# 3. How Cron Works

## a) Cron Uses a Background Service

Cron works using a daemon (background process) called:

```bash id="2fd9m5"
crond
```

or:

```bash id="c1kx6g"
cron
```

depending on Linux distribution.

---

## b) Cron Stores Jobs in Crontab

Scheduled tasks are stored inside:

```text id="rtwup8"
crontab
```

---

## c) Cron Daemon Executes Tasks Automatically

The cron daemon continuously checks crontab and executes matching jobs.

---

# 4. Basic Cron Commands

## a) Edit Current User Crontab

```bash id="yzj4k5"
crontab -e
```

---

## b) View Current Cron Jobs

```bash id="4m9b7v"
crontab -l
```

---

## c) Remove Current Cron Jobs

```bash id="6j0i1n"
crontab -r
```

> Removes all cron jobs permanently for current user.

---

# 5. Cron Table (Crontab) Format

## Basic Structure

```bash id="ms7lfj"
* * * * * command
```

---

# Cron Field Breakdown

| Field   | Meaning      |
| ------- | ------------ |
| 1st `*` | Minute       |
| 2nd `*` | Hour         |
| 3rd `*` | Day of Month |
| 4th `*` | Month        |
| 5th `*` | Day of Week  |

---

# Visual Representation

```text id="jlwm7t"
* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of Week
│ │ │ └──── Month
│ │ └────── Day of Month
│ └──────── Hour
└────────── Minute
```

---

# Special Symbols

| Symbol | Meaning         |
| ------ | --------------- |
| `*`    | Every value     |
| `,`    | Multiple values |
| `-`    | Range           |
| `/`    | Step interval   |

---

# 6. Cron Scheduling Examples

## a) Run Daily at Midnight

```bash id="xod9bl"
0 0 * * * /path/script.sh
```

---

## b) Run on 1st Day of Every Month at 8:30 AM

```bash id="w13d6j"
30 8 1 * * /path/script.sh
```

---

## c) Run Every 15 Minutes

```bash id="imrtd6"
*/15 * * * * /path/script.sh
```

---

## d) Run Monday to Friday at 2:15 PM

```bash id="x3pjlwm"
15 14 * * 1-5 /path/script.sh
```

---

## e) Run in February on 10th, 20th, and 30th at 8 PM

```bash id="6cnklv"
0 20 10,20,30 2 * /path/script.sh
```

---

# 7. Special Cron Strings

## a) Run at Startup

```bash id="sh8uwq"
@reboot command
```

---

## b) Run Yearly

```bash id="r0szy0"
@yearly command
```

or

```bash id="jtvpaz"
@annually command
```

Equivalent to:

```bash id="1ltwxl"
0 0 1 1 *
```

---

## c) Run Monthly

```bash id="kccr50"
@monthly command
```

Equivalent to:

```bash id="r66nwo"
0 0 1 * *
```

---

## d) Run Weekly

```bash id="jlwmta"
@weekly command
```

Equivalent to:

```bash id="52t9df"
0 0 * * 0
```

---

## e) Run Daily

```bash id="skq49q"
@daily command
```

or

```bash id="jlwm0z"
@midnight command
```

Equivalent to:

```bash id="90g6p1"
0 0 * * *
```

---

## f) Run Hourly

```bash id="cbwvf7"
@hourly command
```

Equivalent to:

```bash id="0kz4qh"
0 * * * *
```

---

# 8. Redirecting Cron Output

## a) Save Output to File

```bash id="j2w6wg"
* * * * * /bin/ls /home/folder > /tmp/list.txt
```

---

## b) Append Output

```bash id="bjlwm8"
* * * * * command >> output.log
```

---

## c) Redirect Errors

```bash id="2x7mhl"
* * * * * command 2> errors.log
```

---

## d) Redirect Output and Errors Together

```bash id="nb20a0"
* * * * * command > output.log 2>&1
```

---

# 9. Environment Variables in Cron

Cron runs with limited environment variables.

Commands may fail because:

* PATH is missing,
* Python environment is missing,
* binaries are not found.

---

## Set PATH Variable

Add at top of crontab:

```bash id="a97dpr"
PATH=/usr/bin:/bin:/usr/local/bin
```

---

## Example

```bash id="if0kzs"
PATH=/usr/bin:/bin

*/5 * * * * python3 /home/user/script.py
```

---

# 10. Cron Logs

## Common Log Locations

### RHEL / CentOS

```bash id="odjlwm"
/var/log/cron
```

---

### Ubuntu / Debian

```bash id="js8cjl"
/var/log/syslog
```

---

## View Cron Logs

```bash id="jlwm6v"
grep CRON /var/log/syslog
```

---

# 11. Task Scheduling using `at` Command

> The `at` command is also used for task scheduling.
>
> Unlike cron:
>
> * cron is mainly for repetitive tasks,
> * `at` is mainly for one-time scheduling.

---

# Why Use `at`?

`at` is simpler when:

* you want to run a command only once,
* you do not want repetitive scheduling,
* temporary scheduling is needed.

---

# Basic Syntax

## a) Schedule Task at Specific Time

```bash id="jlwm3x"
at HH:MM
```

### Example

```bash id="0s0a4q"
at 21:00
```

> Schedules task for 9 PM.

After entering command:

```text id="ffhtbm"
at>
```

Now type commands:

```bash id="2h1dtt"
backup.sh
```

Press:

```text id="9cw4v3"
CTRL + D
```

to save and exit.

---

# Listing Scheduled `at` Jobs

## b) View Scheduled Jobs

```bash id="jlwm98"
atq
```

> Shows:
>
> * job ID,
> * scheduled time,
> * queue information.

---

# Remove Scheduled Job

## c) Delete Job

```bash id="jlwm20"
atrm <jobid>
```

### Example

```bash id="jlwm15"
atrm 5
```

---

# `at` Command Examples

## a) Schedule Job at 4:45 AM

```bash id="9z0i2n"
at 4:45 AM
```

---

## b) Schedule Job at 4 PM After 4 Days

```bash id="jlwm9z"
at 4PM +4 days
```

---

## c) Schedule Task After 5 Hours

```bash id="cjlwm2"
at now +5 hours
```

---

## d) Schedule at 9 AM Tomorrow

```bash id="jlwm1f"
at 9:00 AM tomorrow
```

---

## e) Schedule at 10 AM Next Month

```bash id="s5o1p2"
at 10:00 AM next month
```

---

## f) Schedule on Specific Date

```bash id="jlwm28"
at 10:00 AM Nov 30
```

---

# Important Notes about `at`

* `at` is generally used for one-time execution.
* `cron` is better for repetitive scheduling.
* `atq` lists pending jobs.
* `atrm` removes scheduled jobs.
* Commands execute in background.
* Some systems require `atd` service running.

---

# 12. Practical Cybersecurity / Automation Examples

## a) Run IOC Checker Every Hour

```bash id="l8fjlwm"
0 * * * * python3 /opt/ioc_checker.py
```

---

## b) Backup Logs Daily

```bash id="3wt5j0"
0 1 * * * tar -czf /backup/logs.tar.gz /var/log
```

---

## c) Monitor Failed SSH Logins Every 5 Minutes

```bash id="2jlwmf"
*/5 * * * * grep "Failed password" /var/log/auth.log >> /tmp/ssh_failed.log
```

---

## d) Run Nmap Scan Every Sunday

```bash id="njlwmv"
0 2 * * 0 nmap -sV 192.168.1.0/24
```

---

## e) Delete Temporary Files Daily

```bash id="8bcjlwm"
0 0 * * * rm -rf /tmp/*
```

---

# Important Notes

* Cron jobs execute automatically in background.
* Always use full file paths.
* Cron uses minimal environment variables.
* Use output redirection for debugging.
* `crontab -e` edits jobs.
* `crontab -l` lists jobs.
* `crontab -r` removes jobs permanently.
* `at` is useful for one-time scheduling.

---

*End of Cron Job / Task Scheduling Notes*
