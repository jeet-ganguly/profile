# Computer Networks — TCP Connection Establishment

> TCP (Transmission Control Protocol) is a connection-oriented protocol.
>
> Before transmitting data, TCP establishes a connection between the sender and receiver using a process called the **Three-Way Handshake**.
>
> This process synchronizes both hosts and ensures reliable communication.

These notes cover:

* Why Connection Establishment is Needed
* TCP Three-Way Handshake
* SYN and ACK Flags
* Initial Sequence Numbers
* State Transitions
* Why Three Steps are Required
* Simultaneous Open
* TCP Fast Open
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why TCP Connection Establishment is Needed](#2-why-tcp-connection-establishment-is-needed)
* [3. TCP Three-Way Handshake](#3-tcp-three-way-handshake)
* [4. Step 1 — SYN](#4-step-1--syn)
* [5. Step 2 — SYN + ACK](#5-step-2--syn--ack)
* [6. Step 3 — ACK](#6-step-3--ack)
* [7. Initial Sequence Numbers (ISN)](#7-initial-sequence-numbers-isn)
* [8. TCP States During Connection Establishment](#8-tcp-states-during-connection-establishment)
* [9. Why Three-Way Handshake is Needed](#9-why-three-way-handshake-is-needed)
* [10. Simultaneous Open](#10-simultaneous-open)
* [11. TCP Fast Open](#11-tcp-fast-open)
* [12. Cybersecurity Perspective](#12-cybersecurity-perspective)
* [13. Quick Revision Sheet](#13-quick-revision-sheet)

---

# 1. Introduction

TCP is a:

```text
Connection-Oriented Protocol
```

Before exchanging data, both hosts must establish a connection.

Example:

```text
Client
  ↓
TCP Connection
  ↓
Server
```

This process is called:

```text
Three-Way Handshake
```

---

# 2. Why TCP Connection Establishment is Needed

TCP connection establishment ensures:

* Both hosts are alive.
* Sequence numbers are synchronized.
* Receiver is ready.
* Reliable communication is possible.

Without connection establishment:

```text
Lost Packets

Duplicate Packets

Out-of-Order Delivery
```

may occur.

---

# 3. TCP Three-Way Handshake

Connection establishment occurs in three steps:

```text
Client                Server

 SYN      -------->

           <--------   SYN + ACK

 ACK      -------->
```

After completion:

```text
Connection Established
```

---

# 4. Step 1 — SYN

Client initiates the connection.

It sends:

```text
SYN = 1
```

Suppose:

```text
Sequence Number = 1000
```

Packet:

```text
SYN

Seq = 1000
```

State transition:

```text
Client

CLOSED
 ↓
SYN-SENT
```

Purpose:

```text
Request Connection
```

---

# 5. Step 2 — SYN + ACK

Server receives the SYN packet.

Server replies with:

```text
SYN = 1

ACK = 1
```

Suppose server sequence number is:

```text
5000
```

Packet:

```text
SYN + ACK

Seq = 5000

ACK = 1001
```

because:

```text
1000 + 1 = 1001
```

State transition:

```text
Server

LISTEN
 ↓
SYN-RECEIVED
```

Purpose:

```text
Acknowledge Client

and

Request Connection
```

---

# 6. Step 3 — ACK

Client receives SYN+ACK.

It sends:

```text
ACK = 1
```

Packet:

```text
ACK

Seq = 1001

ACK = 5001
```

because:

```text
5000 + 1 = 5001
```

After this:

```text
Client
ESTABLISHED

Server
ESTABLISHED
```

Now:

```text
Data Transfer Begins
```

---

# Complete Three-Way Handshake

```text
Client                          Server
------                          ------

Seq=1000
SYN
-------------------------------------->

                                Seq=5000
                                ACK=1001
                                SYN+ACK
<--------------------------------------

Seq=1001
ACK=5001
ACK
--------------------------------------->

Connection Established
```

---

# 7. Initial Sequence Numbers (ISN)

Each host selects a random:

```text
Initial Sequence Number
```

Example:

Client:

```text
1000
```

Server:

```text
5000
```

Purpose:

* Prevent duplicate packets.
* Improve security.
* Enable reliable communication.

---

# 8. TCP States During Connection Establishment

Client:

```text
CLOSED
   ↓
SYN-SENT
   ↓
ESTABLISHED
```

---

Server:

```text
LISTEN
   ↓
SYN-RECEIVED
   ↓
ESTABLISHED
```

---

# 9. Why Three-Way Handshake is Needed

The three-way handshake ensures:

### Both Hosts Are Reachable

Client verifies:

```text
Server is Alive
```

Server verifies:

```text
Client is Alive
```

---

### Sequence Numbers Are Synchronized

Both sides exchange:

```text
Initial Sequence Numbers
```

for reliable delivery.

---

### Avoid Old Duplicate Packets

Random sequence numbers help distinguish:

```text
Old Connections

from

New Connections
```

---

### Reliable Communication

Ensures:

* Ordered Delivery
* Flow Control
* Error Control

---

# Why Not Two-Way Handshake?

Suppose:

```text
Client --------> SYN
```

Server replies:

```text
SYN + ACK
```

If the reply is lost:

```text
Client thinks:

No Connection

Server thinks:

Connection Established
```

Result:

```text
Half-Open Connection
```

Therefore:

```text
Three Steps Are Necessary
```

---

# 10. Simultaneous Open

Rarely, both hosts send SYN simultaneously.

```text
Host A      SYN      Host B

Host A <--- SYN ---> Host B
```

Both sides exchange:

```text
SYN

SYN+ACK

ACK
```

and eventually reach:

```text
ESTABLISHED
```

---

# 11. TCP Fast Open

Traditional TCP requires:

```text
3 Steps
```

before sending data.

TCP Fast Open allows:

```text
Data in SYN Packet
```

which reduces:

* Latency
* Connection Setup Time

Commonly used in:

* Web Browsers
* CDN Networks

---

# 12. Cybersecurity Perspective

---

## SYN Flood Attack

Attackers continuously send:

```text
SYN Packets
```

without completing the handshake.

Result:

```text
Half-Open Connections
```

consume server resources.

---

Server state:

```text
SYN-RECEIVED
```

fills up.

Eventually:

```text
Denial of Service
```

occurs.

---

## SYN Scan

Nmap uses:

```text
Half-Open TCP Connection
```

for port scanning.

Process:

```text
SYN
   ↓
SYN+ACK
   ↓
RST
```

Connection is never fully established.

---

## Session Hijacking

Attackers attempt to predict:

```text
Sequence Numbers
```

to inject malicious packets.

---

## Firewall Rules

Firewalls commonly inspect:

* SYN Flag
* ACK Flag
* Source Port
* Destination Port

during connection establishment.

---

# 13. Quick Revision Sheet

TCP Connection Setup:

```text
Three-Way Handshake
```

---

Step 1:

```text
SYN

Seq = x
```

---

Step 2:

```text
SYN + ACK

Seq = y

ACK = x+1
```

---

Step 3:

```text
ACK

Seq = x+1

ACK = y+1
```

---

Client States:

```text
CLOSED

↓

SYN-SENT

↓

ESTABLISHED
```

---

Server States:

```text
LISTEN

↓

SYN-RECEIVED

↓

ESTABLISHED
```

---

Important Flags:

```text
SYN

ACK
```

---

Biggest Concept:

```text
TCP uses a Three-Way Handshake
to synchronize sequence numbers
and establish a reliable connection
before transmitting data.
```

---

*End of TCP Connection Establishment Notes*
