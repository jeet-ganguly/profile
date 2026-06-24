# Computer Networks — TCP Connection Termination

> TCP is a connection-oriented protocol. After data transmission is complete, both hosts must properly close the connection.
>
> TCP uses a graceful connection termination process called the **Four-Way Handshake**.
>
> This ensures that all pending data is delivered before the connection is closed.

These notes cover:

* Why TCP Connection Termination is Needed
* TCP Four-Way Handshake
* FIN and ACK Flags
* Half Close Concept
* TCP States During Termination
* TIME_WAIT State
* Simultaneous Close
* Abortive Termination
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why TCP Connection Termination is Needed](#2-why-tcp-connection-termination-is-needed)
* [3. TCP Four-Way Handshake](#3-tcp-four-way-handshake)
* [4. Step 1 — FIN](#4-step-1--fin)
* [5. Step 2 — ACK](#5-step-2--ack)
* [6. Step 3 — FIN](#6-step-3--fin)
* [7. Step 4 — ACK](#7-step-4--ack)
* [8. Why Four Steps are Required](#8-why-four-steps-are-required)
* [9. Half Close Concept](#9-half-close-concept)
* [10. TCP States During Termination](#10-tcp-states-during-termination)
* [11. TIME_WAIT State](#11-time_wait-state)
* [12. Simultaneous Close](#12-simultaneous-close)
* [13. Abortive Termination (RST)](#13-abortive-termination-rst)
* [14. Cybersecurity Perspective](#14-cybersecurity-perspective)
* [15. Quick Revision Sheet](#15-quick-revision-sheet)

---

# 1. Introduction

Once data transmission is complete:

```text
Client
   ↔
Server
```

the TCP connection must be closed properly.

TCP uses:

```text
Four-Way Handshake
```

for graceful connection termination.

---

# 2. Why TCP Connection Termination is Needed

Connection termination ensures:

* All pending data is delivered.
* Resources are released.
* Buffers are cleared.
* No data is lost.

Without proper termination:

```text
Half-Open Connections

Resource Leaks

Lost Data
```

may occur.

---

# 3. TCP Four-Way Handshake

Connection termination usually requires:

```text
Client                Server

 FIN      -------->

           <--------   ACK

           <--------   FIN

 ACK      -------->
```

After completion:

```text
Connection Closed
```

---

# 4. Step 1 — FIN

Suppose the client wants to close the connection.

Client sends:

```text
FIN = 1
```

Packet:

```text
FIN

Seq = X
```

Meaning:

```text
I have finished sending data.
```

Client state changes:

```text
ESTABLISHED
      ↓
FIN_WAIT_1
```

---

# 5. Step 2 — ACK

Server receives FIN.

Server acknowledges it.

Packet:

```text
ACK

ACK = X + 1
```

Meaning:

```text
I received your FIN.
```

Server state:

```text
ESTABLISHED
      ↓
CLOSE_WAIT
```

Client state:

```text
FIN_WAIT_1
      ↓
FIN_WAIT_2
```

---

# 6. Step 3 — FIN

Server may still have data to send.

After finishing its transmission:

```text
Server
```

sends:

```text
FIN = 1
```

Packet:

```text
FIN

Seq = Y
```

Meaning:

```text
I have also finished sending data.
```

Server state:

```text
CLOSE_WAIT
      ↓
LAST_ACK
```

---

# 7. Step 4 — ACK

Client receives server's FIN.

Client replies:

```text
ACK = Y + 1
```

Packet:

```text
ACK
```

Client state:

```text
FIN_WAIT_2
      ↓
TIME_WAIT
```

Server state:

```text
LAST_ACK
      ↓
CLOSED
```

After TIME_WAIT expires:

```text
Client
```

also enters:

```text
CLOSED
```

Connection is fully terminated.

---

# Complete Four-Way Handshake

```text
Client                          Server
------                          ------

FIN
Seq = X
-------------------------------------->

                                ACK
                                ACK=X+1
<--------------------------------------

                                FIN
                                Seq = Y
<--------------------------------------

ACK
ACK=Y+1
-------------------------------------->

Connection Closed
```

---

# 8. Why Four Steps are Required

Unlike connection establishment:

```text
Three-Way Handshake
```

TCP termination uses:

```text
Four-Way Handshake
```

because:

```text
Closing Connection

and

Acknowledging Connection Close

are separate operations.
```

The server may still need to transmit data after acknowledging the client's FIN.

---

## Example

Client:

```text
Finished Sending Data
```

Server:

```text
Still Sending Remaining Data
```

Therefore:

```text
ACK

and

FIN
```

cannot always be combined.

---

# 9. Half Close Concept

TCP supports:

```text
Half Close
```

which means:

```text
One Direction Closed

Other Direction Open
```

---

## Example

Client:

```text
No More Data To Send
```

Server:

```text
Still Sending Data
```

Communication becomes:

```text
Client   ←   Server
```

until server finishes.

---

## Benefit

Allows:

* File Transfers
* Streaming Applications
* Graceful Shutdown

---

# 10. TCP States During Termination

## Client Side

```text
ESTABLISHED
      ↓
FIN_WAIT_1
      ↓
FIN_WAIT_2
      ↓
TIME_WAIT
      ↓
CLOSED
```

---

## Server Side

```text
ESTABLISHED
      ↓
CLOSE_WAIT
      ↓
LAST_ACK
      ↓
CLOSED
```

---

# Common Termination States

| State      | Meaning                    |
| ---------- | -------------------------- |
| FIN_WAIT_1 | FIN Sent                   |
| FIN_WAIT_2 | FIN Acknowledged           |
| CLOSE_WAIT | FIN Received               |
| LAST_ACK   | Waiting for Final ACK      |
| TIME_WAIT  | Waiting Before Final Close |
| CLOSED     | Connection Terminated      |

---

# 11. TIME_WAIT State

After sending the final ACK:

```text
Client
```

enters:

```text
TIME_WAIT
```

---

## Why TIME_WAIT Exists

Purpose:

```text
Ensure Final ACK Reaches Server
```

and

```text
Remove Delayed Duplicate Packets
```

from the network.

---

## Typical Duration

```text
2 × MSL
```

where:

```text
MSL
=
Maximum Segment Lifetime
```

Commonly:

```text
30 - 120 Seconds
```

depending on the operating system.

---

# Why TIME_WAIT is Important

Without TIME_WAIT:

```text
Old TCP Packets
```

may interfere with:

```text
New Connections
```

using the same ports.

---

# 12. Simultaneous Close

Sometimes both hosts send:

```text
FIN
```

at nearly the same time.

Example:

```text
Host A           Host B

 FIN -------->

      <-------- FIN
```

Both sides acknowledge each other.

Eventually:

```text
TIME_WAIT
```

and then:

```text
CLOSED
```

are reached.

---

# 13. Abortive Termination (RST)

TCP can also terminate immediately using:

```text
RST
```

flag.

---

## Purpose

Used when:

* Invalid Connection
* Unexpected Packet
* Application Crash
* Immediate Connection Reset

---

## Example

```text
RST = 1
```

Connection is terminated instantly.

---

## Difference

Graceful Close:

```text
FIN
```

ensures pending data is delivered.

---

Abortive Close:

```text
RST
```

discards pending data and closes immediately.

---

# FIN vs RST

| Feature         | FIN | RST            |
| --------------- | --- | -------------- |
| Graceful Close  | Yes | No             |
| Data Delivered  | Yes | Not Guaranteed |
| Normal Shutdown | Yes | No             |
| Immediate Close | No  | Yes            |

---

# 14. Cybersecurity Perspective

---

## FIN Scan

Attackers may use:

```text
FIN Packets
```

to bypass simple firewalls.

Tools:

* Nmap

---

## RST Injection Attack

Attackers inject:

```text
RST Packets
```

to terminate legitimate sessions.

Result:

```text
Connection Reset
```


---

## DoS Attacks

Large numbers of:

```text
FIN

RST
```

packets may be used to disrupt services.

---

## Incident Response

Security analysts frequently inspect:

* FIN Flags
* RST Flags
* TCP State Transitions
* TIME_WAIT Connections

during investigations.

---

# 15. Quick Revision Sheet

TCP Connection Close:

```text
Four-Way Handshake
```

---

Step 1:

```text
FIN
```

---

Step 2:

```text
ACK
```

---

Step 3:

```text
FIN
```

---

Step 4:

```text
ACK
```

---

Client States:

```text
ESTABLISHED

↓

FIN_WAIT_1

↓

FIN_WAIT_2

↓

TIME_WAIT

↓

CLOSED
```

---

Server States:

```text
ESTABLISHED

↓

CLOSE_WAIT

↓

LAST_ACK

↓

CLOSED
```

---

Important Flags:

```text
FIN

ACK

RST
```

---

TIME_WAIT Purpose:

```text
Ensure Final ACK Delivery

Remove Delayed Packets
```

---

Biggest Concept:

```text
TCP uses a Four-Way Handshake
to gracefully terminate a connection
while ensuring all transmitted data
is delivered successfully.
```

---

*End of TCP Connection Termination Notes*
