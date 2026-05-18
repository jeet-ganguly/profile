# Linux Ping, Netstat & Traceroute Notes

> These commands are essential Linux networking tools used for:
>
> * Connectivity testing
> * Network troubleshooting
> * Latency analysis
> * Route tracking
> * Socket and port analysis

---

# Table of Contents

1. [PING Command](#1-ping-command)
2. [How Ping Works](#2-how-ping-works)
3. [PING Commands and Options](#3-ping-commands-and-options)
4. [NETSTAT Command](#4-netstat-command)
5. [Netstat Commands and Options](#5-netstat-commands-and-options)
6. [TRACEROUTE Command](#6-traceroute-command)
7. [Traceroute Commands and Options](#7-traceroute-commands-and-options)
8. [Practical Cybersecurity Examples](#8-practical-cybersecurity-examples)

---

# 1. PING Command

> `ping` is used to test network connectivity and reachability.

---

## What PING Can Do

* Check network connectivity
* Check internet connection
* Verify network interface operation
* Measure latency
* Verify DNS resolution

---

# 2. How Ping Works

Ping sends:

```text id="j10smk"
ICMP Echo Request
```

to destination.

If destination is reachable:

```text id="k63tds"
ICMP Echo Reply
```

is returned.

---

## Flow

```text id="syjlwm"
Client --------ICMP Request-------> Server
Client <-------ICMP Reply---------- Server
```

---

# 3. PING Commands and Options

## a) Basic Ping

Using IP:

```bash id="ik9uwu"
ping <ip-address>
```

Using domain:

```bash id="ck5civ"
ping <domain-name>
```

Example:

```bash id="8ij10e"
ping google.com
```

> Domain ping also checks DNS resolution.

---

## b) Send Only 3 Packets

```bash id="ty3vyr"
ping -c 3 google.com
```

> `-c` specifies count.

---

## c) Set Interval Between Packets

```bash id="v20m2a"
ping -i 2 google.com
```

> Sends packet every 2 seconds.

---

## d) Flood Ping

```bash id="ptxejw"
ping -f google.com
```

> Sends packets as fast as possible.

---

## e) Show Summary Only

```bash id="jlwm5c"
ping -c 3 -q google.com
```

> `-q` shows summary output only.

---

## f) Change Packet Size

```bash id="vk13tx"
ping -s 500 google.com
```

> Sends ICMP packets of size 500 bytes.

---

## g) Audible Alert

```bash id="jlwmr7"
ping -a google.com
```

> Generates sound when host responds.

---

## h) Test Network Interface Card

```bash id="jlwm0a"
ping localhost
```

or:

```bash id="jlwmz3"
ping 127.0.0.1
```

If localhost ping fails:

* loopback interface issue
* possible network stack problem

---

# Useful Ping Output Fields

```text id="zhxjlwm"
64 bytes from google.com:
icmp_seq=1
ttl=118
time=24 ms
```

| Field    | Meaning         |
| -------- | --------------- |
| bytes    | Packet size     |
| icmp_seq | Packet number   |
| ttl      | Time To Live    |
| time     | Round trip time |

---

# 4. NETSTAT Command

> `netstat` is a command-line networking utility used to display:

* Network connections
* Routing tables
* Interface statistics
* TCP connections
* UDP connections
* Protocol statistics

---

## Useful Combination

```bash id="vjlwmx"
netstat -plantu
```

Shows:

* TCP
* UDP
* Listening services
* PID
* Program name

---

# 5. Netstat Commands and Options

## a) Search Connection by Port/IP

```bash id="1jlwmm"
netstat -putan | grep <port/ip>
```

---

## b) View All Sockets

```bash id="jlwm8k"
netstat -a
```

Shows:

* network sockets
* UNIX sockets

---

## c) View TCP Connections

```bash id="zjlwm4"
netstat -at
```

---

## d) View TCP IPv6 Connections

```bash id="2jlwmw"
netstat -6at
```

---

## e) View UDP Connections

```bash id="jlwm00"
netstat -au
```

---

## f) View Listening Ports

```bash id="jlwmm8"
netstat -l
```

---

## g) Show Numerical Addresses

```bash id="fjlwm7"
netstat -ln
```

Avoids DNS resolution.

---

## h) Show Program PID

```bash id="jlwm44"
netstat -p
```

Shows process using port.

---

## i) View Routing Table

```bash id="8jlwmv"
netstat -r
```

---

## j) Check Connections from Specific IP

```bash id="ljlwm3"
netstat -an | grep <ip>
```

---

## k) Show Network Interfaces

```bash id="jlwm6f"
netstat -i
```

---

## l) View Protocol Statistics

```bash id="xjlwm1"
netstat -s
```

Shows:

* TCP statistics
* UDP statistics
* ICMP statistics

---

# Common Netstat Flags

| Flag | Meaning         |
| ---- | --------------- |
| `-a` | All connections |
| `-t` | TCP             |
| `-u` | UDP             |
| `-l` | Listening       |
| `-n` | Numeric         |
| `-p` | PID             |
| `-r` | Routing         |
| `-i` | Interfaces      |
| `-s` | Statistics      |

---

# Note

Modern Linux systems often use:

```bash id="jlwmj9"
ss
```

instead of:

```bash id="6jlwm2"
netstat
```

because `ss` is faster.

---

# 6. TRACEROUTE Command

> Traceroute tracks the path packets take from your machine to destination.

Used for:

* path analysis
* identifying routing issues
* latency investigation
* network troubleshooting

---

# Basic Working

Traceroute discovers:

```text id="jlwmu5"
Computer → Router → ISP → Intermediate Routers → Destination
```

---

# 7. Traceroute Commands and Options

## a) Basic Traceroute

```bash id="jlwm7v"
traceroute <ip>
```

Example:

```bash id="6jlwmn"
traceroute google.com
```

---

## b) Change Packet Length

```bash id="sjlwm4"
traceroute google.com 100
```

> Uses packet size 100.

---

## c) Change Number of Queries Per Hop

Default:

```text id="jlwmj0"
3 packets per hop
```

Change:

```bash id="mjlwm9"
traceroute -q 5 google.com
```

---

## d) Change Port Number

Default port:

```text id="4jlwmz"
33434
```

Change:

```bash id="xjlwm0"
traceroute -p 8080 google.com
```

---

## e) Use IPv4 or IPv6

IPv4:

```bash id="wjlwmy"
traceroute -4 google.com
```

IPv6:

```bash id="tjlwm5"
traceroute -6 google.com
```

---

## f) Route Through Gateway

```bash id="jlwmk3"
traceroute -g <gateway-ip> google.com
```

---

# 8. Practical Cybersecurity Examples

## a) Detect High Network Latency

```bash id="jlwmx7"
ping -c 10 google.com
```

---

## b) Check Whether Internal Server is Reachable

```bash id="8jlwm1"
ping 192.168.1.10
```

---

## c) Identify Which Program Uses Port 4444

```bash id="njlwm4"
netstat -plant | grep 4444
```

---

## d) Check Reverse Shell Connections

```bash id="djlwm2"
netstat -antp
```

---

## e) Investigate Packet Route to Server

```bash id="jjlwm8"
traceroute company.com
```

---

# Notes

* Ping uses ICMP packets.
* Ping measures latency and connectivity.
* Netstat displays sockets and network statistics.
* Traceroute shows packet path.
* `netstat -plantu` is a commonly used combination.
* `ss` replaces netstat on many systems.
* `localhost` tests loopback interface.

---

*End of Linux Networking Notes*
