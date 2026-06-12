# Computer Networks — IPv6 Header

> Every IPv6 packet contains a header that stores control information required for packet forwarding and communication.
>
> Compared to IPv4, the IPv6 header is simpler and more efficient. Several fields present in IPv4 were removed or moved to extension headers to improve routing performance.

These notes cover:

* What is an IPv6 Header
* IPv6 Header Structure
* Header Fields
* Traffic Class
* Flow Label
* Payload Length
* Next Header
* Hop Limit
* Source and Destination Address
* Extension Headers
* IPv4 Header vs IPv6 Header
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why IPv6 Header is Needed](#2-why-ipv6-header-is-needed)
* [3. IPv6 Header Structure](#3-ipv6-header-structure)
* [4. Version Field](#4-version-field)
* [5. Traffic Class](#5-traffic-class)
* [6. Flow Label](#6-flow-label)
* [7. Payload Length](#7-payload-length)
* [8. Next Header](#8-next-header)
* [9. Hop Limit](#9-hop-limit)
* [10. Source Address](#10-source-address)
* [11. Destination Address](#11-destination-address)
* [12. Extension Headers](#12-extension-headers)
* [13. IPv4 Header vs IPv6 Header](#13-ipv4-header-vs-ipv6-header)
* [14. Cybersecurity Perspective](#14-cybersecurity-perspective)
* [15. Quick Revision Sheet](#15-quick-revision-sheet)

---

# 1. Introduction

An IPv6 packet consists of:

```text
IPv6 Header
+
Payload(Data)
```

The header contains control information.

The payload contains user data.

---

# 2. Why IPv6 Header is Needed

Without the header:

```text
Routers would not know

Where packet came from

Where packet should go

Which protocol is being used

How long packet can travel
```

The IPv6 header provides all this information.

---

# 3. IPv6 Header Structure

Unlike IPv4:

```text
Variable Length Header
```

IPv6 uses:

```text
Fixed Length Header
```

Header Size:

```text
40 Bytes
```

---

## IPv6 Header Layout

```text
+---------+-------------+----------------+
| Version | TrafficCls  | Flow Label     |
+---------+-------------+----------------+
| Payload Length | Next Header | HopLimit |
+-----------------------------------------+
|                                         |
|         Source Address (128 Bits)       |
|                                         |
+-----------------------------------------+
|                                         |
|      Destination Address (128 Bits)     |
|                                         |
+-----------------------------------------+
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

Value for IPv6:

```text
6
```

---

## Example

```text
Version = 6
```

Router immediately recognizes:

```text
This is an IPv6 Packet
```

---

# 5. Traffic Class

Size:

```text
8 Bits
```

Purpose:

```text
Packet Priority Information
```

Used for:

* Voice Traffic
* Video Streaming
* QoS

---

## Example

Voice packets may receive higher priority than:

```text
File Download Traffic
```

---

## Equivalent IPv4 Field

```text
Type of Service (ToS)
```

---

# 6. Flow Label

Size:

```text
20 Bits
```

Purpose:

```text
Identify Packet Flows
```

A flow represents:

```text
Series of packets
belonging to the same
communication session
```

---

## Example

Video Streaming:

```text
Client
 ↓
Multiple Packets
 ↓
Same Flow Label
```

Routers can process them more efficiently.

---

## Advantages

* Better QoS
* Efficient Routing
* Improved Multimedia Performance

---

# 7. Payload Length

Size:

```text
16 Bits
```

Purpose:

```text
Length of Payload Data
```

Unlike IPv4:

```text
Payload Length
```

does not include the header size.

---

## Example

Data Size:

```text
1000 Bytes
```

Payload Length:

```text
1000
```

Header Size:

```text
40 Bytes
```

Total Packet Size:

```text
1040 Bytes
```

---

# 8. Next Header

Size:

```text
8 Bits
```

Purpose:

```text
Identifies the next protocol
or extension header
```

---

## Common Values

| Value | Protocol            |
| ----- | ------------------- |
| 6     | TCP                 |
| 17    | UDP                 |
| 58    | ICMPv6              |
| 43    | Routing Header      |
| 44    | Fragment Header     |
| 60    | Destination Options |

---

## Example

```text
Next Header = 6
```

Meaning:

```text
TCP Segment follows
```

---

## Equivalent IPv4 Field

```text
Protocol Field
```

---

# 9. Hop Limit

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
Hop Limit = 64
```

Router 1:

```text
63
```

Router 2:

```text
62
```

Router 3:

```text
61
```

---

When value becomes:

```text
0
```

the packet is discarded.

---

## Equivalent IPv4 Field

```text
TTL (Time To Live)
```

---

# 10. Source Address

Size:

```text
128 Bits
```

Purpose:

```text
Identify Sender
```

Example:

```text
2001:db8::10
```

Used for:

* Replies
* Session Management

---

# 11. Destination Address

Size:

```text
128 Bits
```

Purpose:

```text
Identify Receiver
```

Example:

```text
2001:db8::20
```

Routers mainly inspect:

```text
Destination Address
```

to forward packets.

---

# 12. Extension Headers

IPv6 moved many optional fields outside the main header.

These are called:

```text
Extension Headers
```

---

## Why Extension Headers?

Benefits:

* Faster Routing
* Simpler Header
* Better Performance

---

## Common Extension Headers

### Hop-by-Hop Options Header

Processed by every router.

---

### Routing Header

Contains routing information.

---

### Fragment Header

Handles packet fragmentation.

Unlike IPv4:

```text
Routers do NOT fragment packets
```

Only sender performs fragmentation.

---

### Destination Options Header

Processed only by destination node.

---

### Authentication Header (AH)

Provides:

* Integrity
* Authentication

Used with IPsec.

---

### Encapsulating Security Payload (ESP)

Provides:

* Encryption
* Confidentiality

Used with IPsec.

---

# 13. IPv4 Header vs IPv6 Header

| Feature        | IPv4                 | IPv6              |
| -------------- | -------------------- | ----------------- |
| Header Size    | Variable             | Fixed (40 Bytes)  |
| Address Size   | 32 Bits              | 128 Bits          |
| Checksum       | Present              | Removed           |
| Fragmentation  | Routers Can Fragment | Sender Only       |
| Options        | Inside Header        | Extension Headers |
| TTL            | TTL                  | Hop Limit         |
| Protocol Field | Protocol             | Next Header       |

---

## Major Simplifications in IPv6

Removed:

* Header Checksum
* Identification Field
* Flags Field
* Fragment Offset

Moved optional functions to:

```text
Extension Headers
```

---

# 14. Cybersecurity Perspective

---

## Extension Header Abuse

Attackers may manipulate:

```text
Routing Header
```

or

```text
Fragment Header
```

to evade:

* Firewalls
* IDS
* IPS

---

## Flow Label Abuse

Malware may use unusual:

```text
Flow Label Values
```

for covert communication.

---

## Fragmentation Attacks

Improper handling of:

```text
Fragment Header
```

can bypass security devices.

---

## IPv6 Blind Spots

Many organizations inspect:

```text
IPv4 Traffic
```

but neglect:

```text
IPv6 Traffic
```

creating:

* Security Gaps
* Undetected Traffic
* Policy Bypass

---

# 15. Quick Revision Sheet

Header Size:

```text
40 Bytes
```

---

Important Fields:

```text
Version

Traffic Class

Flow Label

Payload Length

Next Header

Hop Limit

Source Address

Destination Address
```

---

Equivalent Fields

```text
TTL
↓
Hop Limit

Protocol
↓
Next Header

ToS
↓
Traffic Class
```

---

Common Next Header Values

```text
6   → TCP

17  → UDP

58  → ICMPv6

43  → Routing Header

44  → Fragment Header
```

---

Biggest Concept:

```text
IPv6 uses a fixed 40-byte header
with a simplified design and
moves optional functionality
to Extension Headers for
better performance and scalability.
```

---

*End of IPv6 Header Notes*
