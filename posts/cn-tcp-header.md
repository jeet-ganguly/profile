# Computer Networks — TCP Header

> TCP (Transmission Control Protocol) is a connection-oriented and reliable transport layer protocol.
>
> Every TCP segment contains a TCP Header which stores control information required for reliable communication, sequencing, flow control, and error detection.
>
> The TCP header allows TCP to provide ordered and reliable delivery of data.

These notes cover:

* What is a TCP Header
* TCP Header Structure
* Source and Destination Ports
* Sequence Number
* Acknowledgment Number
* Header Length
* Flags
* Window Size
* Checksum
* Urgent Pointer
* Options Field
* MSS
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why TCP Header is Needed](#2-why-tcp-header-is-needed)
* [3. TCP Header Structure](#3-tcp-header-structure)
* [4. Source Port Number](#4-source-port-number)
* [5. Destination Port Number](#5-destination-port-number)
* [6. Sequence Number](#6-sequence-number)
* [7. Acknowledgment Number](#7-acknowledgment-number)
* [8. Header Length (Data Offset)](#8-header-length-data-offset)
* [9. Reserved Bits](#9-reserved-bits)
* [10. TCP Flags](#10-tcp-flags)
* [11. Window Size](#11-window-size)
* [12. Checksum](#12-checksum)
* [13. Urgent Pointer](#13-urgent-pointer)
* [14. Options Field](#14-options-field)
* [15. MTU (Maximum Transmission Unit)](#15-mtu-maximum-transmission-unit)
* [16. MSS (Maximum Segment Size)](#16-mss-maximum-segment-size)
* [17. Cybersecurity Perspective](#17-cybersecurity-perspective)
* [18. Quick Revision Sheet](#18-quick-revision-sheet)

---

# 1. Introduction

TCP adds a header before transmitting data.

A TCP segment consists of:

```text
TCP Header
+
Application Data
```

Minimum Header Size:

```text
20 Bytes
```

Maximum Header Size:

```text
60 Bytes
```

---

# 2. Why TCP Header is Needed

Without the TCP header:

```text
Sender and receiver would not know:

Which application sent data

Which application should receive data

Packet ordering

Lost packets

Acknowledgments

Flow control information
```

TCP header provides all these functions.

---

# 3. TCP Header Structure

```text
0                   15                  31
+----------------+----------------+
| Source Port    | Destination Port|
+----------------------------------+
|         Sequence Number          |
+----------------------------------+
|      Acknowledgment Number       |
+----------------------------------+
|Data|Res| Flags | Window Size     |
|Off |   |       |                 |
+----------------------------------+
| Checksum     | Urgent Pointer    |
+----------------------------------+
|       Options (Optional)         |
+----------------------------------+
|            Data                  |
+----------------------------------+
```

---

# 4. Source Port Number

Size:

```text
16 Bits
```

Purpose:

```text
Identifies Sender Application
```

Example:

```text
52314
```

Usually an ephemeral port chosen by the operating system.

---

## Example

```text
Browser

192.168.1.10:52314
```

communicates with:

```text
Web Server

8.8.8.8:80
```

---

# 5. Destination Port Number

Size:

```text
16 Bits
```

Purpose:

```text
Identifies Destination Application
```

Examples:

| Port | Service |
| ---- | ------- |
| 21   | FTP     |
| 22   | SSH     |
| 23   | Telnet  |
| 25   | SMTP    |
| 53   | DNS     |
| 80   | HTTP    |
| 443  | HTTPS   |

---

# 6. Sequence Number

Size:

```text
32 Bits
```

Purpose:

```text
Maintain Packet Order
```

and

```text
Detect Missing Segments
```

---

## Example

Suppose data is divided into:

```text
Segment 1

Segment 2

Segment 3
```

Sequence Numbers:

```text
1000

2000

3000
```

Receiver uses them to arrange packets correctly.

---

## Why Needed?

Because IP packets may arrive:

```text
Out of Order
```

---

# 7. Acknowledgment Number

Size:

```text
32 Bits
```

Purpose:

```text
Confirm Received Data
```

---

## Example

Sender:

```text
Seq = 1000
```

Receiver replies:

```text
ACK = 2000
```

Meaning:

```text
I have successfully received
everything up to byte 1999.

Send byte 2000 next.
```

---

# 8. Header Length (Data Offset)

Size:

```text
4 Bits
```

Purpose:

```text
Indicates Header Size
```

---

Minimum Value:

```text
20 Bytes
```

Maximum Value:

```text
60 Bytes
```

---

## Why Needed?

Because options field makes the TCP header variable in size.

Receiver needs to know:

```text
Where payload begins
```

---

# 9. Reserved Bits

Size:

```text
3 Bits
```

Purpose:

```text
Reserved for Future Use
```

Normally:

```text
Set to Zero
```

---

# 10. TCP Flags

Flags are control bits used to manage TCP connections.

---

## SYN

Meaning:

```text
Synchronize
```

Purpose:

```text
Start TCP Connection
```

Used in:

```text
Three-Way Handshake
```

---

## ACK

Meaning:

```text
Acknowledgment
```

Purpose:

```text
Confirm Receipt
```

---

## FIN

Meaning:

```text
Finish
```

Purpose:

```text
Terminate Connection
```

---

## RST

Meaning:

```text
Reset
```

Purpose:

```text
Abort Connection
```

---

## PSH

Meaning:

```text
Push
```

Purpose:

```text
Deliver Data Immediately
```

---

## URG

Meaning:

```text
Urgent
```

Purpose:

```text
Urgent Data Present
```

---

## Flag Summary

| Flag | Meaning              |
| ---- | -------------------- |
| SYN  | Establish Connection |
| ACK  | Acknowledge Data     |
| FIN  | Close Connection     |
| RST  | Reset Connection     |
| PSH  | Immediate Delivery   |
| URG  | Urgent Data          |

---

# 11. Window Size

Size:

```text
16 Bits
```

Purpose:

```text
Flow Control
```

It specifies:

```text
How much data
receiver can accept
without acknowledgment
```

---

## Example

Window Size:

```text
4096 Bytes
```

Sender can transmit:

```text
4096 Bytes
```

before waiting for ACK.

---

## Benefit

Prevents:

```text
Fast Sender
```

from overwhelming:

```text
Slow Receiver
```

---

# 12. Checksum

Size:

```text
16 Bits
```

Purpose:

```text
Error Detection
```

Sender computes checksum.

Receiver recalculates checksum.

---

If:

```text
Values Match
```

packet is valid.

Otherwise:

```text
Packet Corrupted
```

---

# 13. Urgent Pointer

Size:

```text
16 Bits
```

Purpose:

```text
Indicates Urgent Data
```

Used only when:

```text
URG Flag = 1
```

---

Today this field is rarely used.

---

# 14. Options Field

Size:

```text
0–40 Bytes
```

Purpose:

```text
Provide Additional Features
```

---

Common Options:

* MSS
* Window Scaling
* Selective Acknowledgment (SACK)
* Timestamp

---

# 15. MTU (Maximum Transmission Unit)

MTU stands for:

```text
Maximum Transmission Unit
```

It represents:

```text
Maximum size of an entire packet
that can be transmitted over
a network medium without fragmentation.
```

---

## Why MTU is Needed?

Different network technologies have different packet size limits.

Examples:

| Technology | MTU        |
| ---------- | ---------- |
| Ethernet   | 1500 Bytes |
| PPPoE      | 1492 Bytes |
| FDDI       | 4352 Bytes |
| Token Ring | 4464 Bytes |

---

## Ethernet MTU

Most Ethernet networks use:

```text
MTU = 1500 Bytes
```

This includes:

```text
IP Header
+
TCP Header
+
Application Data
```

---

## Example

Suppose:

```text
IP Header = 20 Bytes

TCP Header = 20 Bytes

Application Data = 1460 Bytes
```

Total packet size:

```text
20 + 20 + 1460

= 1500 Bytes
```

which exactly matches Ethernet MTU.

---

## What Happens if Packet Size Exceeds MTU?

Suppose:

```text
Packet Size = 2000 Bytes
```

but network MTU is:

```text
1500 Bytes
```

Then:

```text
Packet
     ↓
Fragmentation
     ↓
Two Smaller Packets
```

may occur.

---

## Problems Caused by Fragmentation

* Increased overhead
* Reduced performance
* Packet loss
* Higher latency

Therefore:

```text
Networks try to avoid fragmentation
whenever possible.
```

---

## Path MTU Discovery (PMTUD)

Path MTU Discovery determines:

```text
Smallest MTU
along the entire route
```

between sender and receiver.

Purpose:

```text
Avoid Fragmentation
```

---

## Example

Suppose route contains:

```text
Router 1 → MTU 1500

Router 2 → MTU 1400

Router 3 → MTU 1300
```

Effective MTU becomes:

```text
1300 Bytes
```

because it is the smallest MTU on the path.

---

# 16. MSS (Maximum Segment Size)

MSS stands for:

```text
Maximum Segment Size
```

It represents:

```text
Maximum amount of
Application Data
inside a TCP segment.
```

---

## Relationship Between MTU and MSS

```text
MTU
│
├── IP Header
│
├── TCP Header
│
└── Application Data (MSS)
```

---

## Formula

```text
MSS = MTU - IP Header - TCP Header
```

---

## Example (IPv4)

Ethernet MTU:

```text
1500 Bytes
```

IPv4 Header:

```text
20 Bytes
```

TCP Header:

```text
20 Bytes
```

Therefore:

```text
MSS = 1500 - 20 - 20

= 1460 Bytes
```

---

## Example (IPv6)

Ethernet MTU:

```text
1500 Bytes
```

IPv6 Header:

```text
40 Bytes
```

TCP Header:

```text
20 Bytes
```

Therefore:

```text
MSS = 1500 - 40 - 20

= 1440 Bytes
```

---

## Why MSS is Needed?

MSS helps:

* Avoid Fragmentation
* Improve Performance
* Reduce Packet Loss
* Increase Transmission Efficiency

---

## Memory Trick

```text
MTU
=
Entire Packet Size

MSS
=
Only Application Data
```

---

# 17. Cybersecurity Perspective

## SYN Flood Attack

Attackers send many:

```text
SYN Packets
```

without completing the handshake.

Result:

```text
Resource Exhaustion
```

---

## RST Injection Attack

Attackers send:

```text
RST Packets
```

to terminate connections.

---

## TCP Session Hijacking

Attackers predict:

```text
Sequence Numbers
```

and inject malicious packets.

---

## Fragmentation Attacks

Improper handling of fragmented packets may allow attackers to:

* Evade IDS/IPS
* Bypass Firewalls
* Hide Malicious Payloads

---

## Commonly Analyzed Fields

* Source Port
* Destination Port
* Sequence Number
* Flags
* Window Size
* MSS
* MTU

---

# 18. Quick Revision Sheet

Header Size:

```text
Minimum = 20 Bytes

Maximum = 60 Bytes
```

---

Important Fields:

```text
Source Port

Destination Port

Sequence Number

Acknowledgment Number

Flags

Window Size

Checksum
```

---

Important Flags:

```text
SYN

ACK

FIN

RST

PSH

URG
```

---

MTU:

```text
Entire Packet Size
```

---

MSS:

```text
Application Data Size
```

---

IPv4 MSS:

```text
1460 Bytes
```

---

IPv6 MSS:

```text
1440 Bytes
```

---

Biggest Concept:

```text
MTU limits the size of
the whole packet.

MSS limits the size of
the TCP payload.
```

---

*End of TCP Header Notes*
