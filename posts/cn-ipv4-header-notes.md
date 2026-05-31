# Computer Networks — IPv4 Header

> Every IPv4 packet contains a header.
>
> The IPv4 Header stores important control information required for routing, packet delivery, fragmentation, and communication across networks.
>
> Routers and networking devices mainly inspect the IPv4 header to determine how packets should be processed and forwarded.

These notes cover:

* What is an IPv4 Header
* Why IPv4 Header is Needed
* IPv4 Header Structure
* Header Fields
* Fragmentation Fields
* TTL
* Protocol Field
* Header Checksum
* Source and Destination IP
* IPv4 Options
* Packet Processing
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why IPv4 Header is Needed](#2-why-ipv4-header-is-needed)
* [3. IPv4 Header Structure](#3-ipv4-header-structure)
* [4. Version Field](#4-version-field)
* [5. IHL Field](#5-ihl-field)
* [6. Type of Service (ToS)](#6-type-of-service-tos)
* [7. Total Length](#7-total-length)
* [8. Identification](#8-identification)
* [9. Flags](#9-flags)
* [10. Fragment Offset](#10-fragment-offset)
* [11. Time To Live (TTL)](#11-time-to-live-ttl)
* [12. Protocol Field](#12-protocol-field)
* [13. Header Checksum](#13-header-checksum)
* [14. Source IP Address](#14-source-ip-address)
* [15. Destination IP Address](#15-destination-ip-address)
* [16. Options Field](#16-options-field)
* [17. IPv4 Packet Processing](#17-ipv4-packet-processing)
* [18. Cybersecurity Perspective](#18-cybersecurity-perspective)
* [19. Quick Revision Sheet](#19-quick-revision-sheet)

---

# 1. Introduction

An IPv4 packet consists of:

```text
IPv4 Header
+
Payload(Data)
```

The header contains control information.

The payload contains actual user data.

Example:

```text
+----------------+
| IPv4 Header    |
+----------------+
| TCP Segment    |
+----------------+
```

---

# 2. Why IPv4 Header is Needed

Without a header:

```text
Routers would not know

Where packet came from
Where packet should go
Which protocol is being used
Whether packet is fragmented
How long packet can travel
```

The IPv4 header provides all this information.

---

# 3. IPv4 Header Structure

Minimum Header Size:

```text
20 Bytes
```

Maximum Header Size:

```text
60 Bytes
```

Basic Structure:

```text
Version
IHL
ToS
Total Length

Identification
Flags
Fragment Offset

TTL
Protocol
Header Checksum

Source Address

Destination Address

Options (Optional)
```

---

# Complete IPv4 Header Layout

```text
0                   15                  31
+--------+--------+----------------------+
|Version |  IHL   | Type of Service      |
+--------+--------+----------------------+
|           Total Length                 |
+----------------------------------------+
|         Identification                 |
+--------+-------------------------------+
| Flags  |      Fragment Offset          |
+--------+-------------------------------+
| TTL    | Protocol | Header Checksum    |
+----------------------------------------+
|           Source IP Address            |
+----------------------------------------+
|        Destination IP Address          |
+----------------------------------------+
|       Options (Optional)               |
+----------------------------------------+
```

---

# 4. Version Field

Size:

```text
4 Bits
```

Purpose:

```text
Identifies IP Version
```

Value for IPv4:

```text
4
```

Value for IPv6:

```text
6
```

Example:

```text
Version = 4
```

Router immediately knows:

```text
This is an IPv4 Packet
```

---

# 5. IHL Field

IHL means:

```text
Internet Header Length
```

Size:

```text
4 Bits
```

Purpose:

```text
Specifies Header Size
```

---

## Why Needed?

Headers may contain:

```text
Options
```

which increase header size.

Receiver must know:

```text
Where payload starts
```

---

## Example

Minimum header:

```text
20 Bytes
```

IHL Value:

```text
5
```

because:

```text
5 × 4 Bytes = 20 Bytes
```

---

# 6. Type of Service (ToS)

Size:

```text
8 Bits
```

Purpose:

```text
Packet Priority Information
```

Used for:

* Voice traffic
* Video traffic
* Normal traffic

---

## Goal

Allow routers to prioritize important packets.

Example:

```text
Voice Call
```

may receive higher priority than:

```text
File Download
```

---

# Modern Equivalent

Today ToS is mainly used through:

```text
DSCP
```

(Differentiated Services Code Point)

---

# 7. Total Length

Size:

```text
16 Bits
```

Purpose:

```text
Total Packet Size
```

Includes:

```text
Header + Data
```

---

## Example

Header:

```text
20 Bytes
```

Data:

```text
1000 Bytes
```

Total Length:

```text
1020 Bytes
```

---

## Maximum Value

```text
65,535 Bytes
```

---

# 8. Identification

Size:

```text
16 Bits
```

Purpose:

```text
Identify Fragments
```

Used when packet fragmentation occurs.

---

## Example

Original Packet:

```text
Packet ID = 500
```

Fragments:

```text
Fragment 1 → ID 500

Fragment 2 → ID 500

Fragment 3 → ID 500
```

Receiver knows:

```text
All belong to same packet
```

---

# 9. Flags

Size:

```text
3 Bits
```

Purpose:

```text
Control Fragmentation
```

---

## Flag 1

Reserved

```text
Must be 0
```

---

## Flag 2

DF Bit

```text
Don't Fragment
```

Value:

```text
1 = Do Not Fragment

0 = Fragment Allowed
```

---

## Flag 3

MF Bit

```text
More Fragments
```

Value:

```text
1 = More Fragments Coming

0 = Last Fragment
```

---

# Example

```text
MF = 1
```

means:

```text
More packet fragments exist
```

---

# 10. Fragment Offset

Size:

```text
13 Bits
```

Purpose:

```text
Indicates Fragment Position
```

during reassembly.

---

## Example

Packet fragmented into:

```text
Fragment 1
Fragment 2
Fragment 3
```

Offset values help receiver place them correctly.

---

## Why Important?

Without Fragment Offset:

```text
Receiver cannot reconstruct
original packet
```

---

# 11. Time To Live (TTL)

Size:

```text
8 Bits
```

Purpose:

```text
Prevent Infinite Loops
```

---

## Example

Packet starts with:

```text
TTL = 64
```

Router 1:

```text
TTL = 63
```

Router 2:

```text
TTL = 62
```

Router 3:

```text
TTL = 61
```

---

## When TTL Becomes Zero

Router drops packet.

Reason:

```text
Prevent endless circulation
```

---

## Cybersecurity Importance

TTL is used by:

* traceroute
* network diagnostics
* threat hunting

---

# 12. Protocol Field

Size:

```text
8 Bits
```

Purpose:

```text
Identifies Next Layer Protocol
```

---

## Common Values

| Value | Protocol |
| ----- | -------- |
| 1     | ICMP     |
| 6     | TCP      |
| 17    | UDP      |
| 89    | OSPF     |

---

## Example

```text
Protocol = 6
```

Meaning:

```text
Payload contains TCP Segment
```

---

# Why Needed?

Network Layer must know:

```text
Which protocol should receive payload
```

---

# 13. Header Checksum

Size:

```text
16 Bits
```

Purpose:

```text
Detect Header Corruption
```

---

## How It Works

Sender:

```text
Calculates checksum
```

Receiver:

```text
Recalculates checksum
```

Comparison:

```text
Match → Valid

Mismatch → Corrupted
```

---

## Important

Checksum protects:

```text
Header Only
```

Not payload.

---

# 14. Source IP Address

Size:

```text
32 Bits
```

Purpose:

```text
Sender Identification
```

Example:

```text
192.168.1.10
```

---

## Why Needed?

Allows destination to know:

```text
Who sent packet
```

and where reply should be sent.

---

# 15. Destination IP Address

Size:

```text
32 Bits
```

Purpose:

```text
Identify Receiver
```

Example:

```text
8.8.8.8
```

---

## Importance

Routers primarily inspect:

```text
Destination IP Address
```

to forward packets.

---

# 16. Options Field

Size:

```text
0–40 Bytes
```

Optional field.

Not present in most packets.

---

## Uses

* Security
* Timestamping
* Route Recording
* Debugging

---

## Why Rarely Used?

Because it:

```text
Increases Processing Overhead
```

Most modern traffic uses:

```text
Standard 20-byte Header
```

---

# 17. IPv4 Packet Processing

When a router receives a packet:

### Step 1

Read Version Field

```text
IPv4 or IPv6?
```

---

### Step 2

Check Header Integrity

```text
Header Checksum
```

---

### Step 3

Check TTL

```text
TTL > 0 ?
```

---

### Step 4

Read Destination IP

```text
Where should packet go?
```

---

### Step 5

Check Routing Table

```text
Best Route?
```

---

### Step 6

Forward Packet

---

# Simplified Router Workflow

```text
Packet Arrives
      ↓
Read Header
      ↓
Verify Checksum
      ↓
Decrease TTL
      ↓
Check Destination IP
      ↓
Routing Table Lookup
      ↓
Forward Packet
```

---

# 18. Cybersecurity Perspective

---

## IP Spoofing

Attacker modifies:

```text
Source IP Field
```

Purpose:

* Hide identity
* Bypass filters
* Launch attacks

---

## TTL Analysis

Security analysts often inspect:

```text
TTL Values
```

to identify:

* Operating Systems
* Network Distance
* Suspicious Traffic

---

## Fragmentation Attacks

Attackers manipulate:

```text
Fragmentation Fields
```

to bypass:

* Firewalls
* IDS
* IPS

---

## Header Manipulation

Malware may alter:

* TTL
* ToS
* Protocol Fields

to evade detection.

---

## Packet Inspection

Security tools analyze:

```text
IPv4 Header
```

to detect:

* Spoofing
* Scanning
* Reconnaissance
* Anomalous Traffic

---

# 19. Quick Revision Sheet

## Minimum Header Size

```text
20 Bytes
```

---

## Maximum Header Size

```text
60 Bytes
```

---

## Important Fields

```text
Version

IHL

Total Length

Identification

Flags

Fragment Offset

TTL

Protocol

Header Checksum

Source IP

Destination IP
```

---

## Protocol Numbers

```text
1  → ICMP

6  → TCP

17 → UDP

89 → OSPF
```

---

## Fragmentation Fields

```text
Identification

Flags

Fragment Offset
```

---

## Most Important Router Fields

```text
Destination IP

TTL

Protocol

Header Checksum
```

---

## Biggest Concept

```text
IPv4 Header contains all
control information needed
for packet routing,
delivery, and processing.
```

---

*End of IPv4 Header Notes*
