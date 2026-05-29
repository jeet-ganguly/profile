# Computer Networks — IP Addressing

> An IP (Internet Protocol) Address is a logical address assigned to a device in a network.
>
> It uniquely identifies a device and allows communication between devices across local networks and the Internet.
>
> Unlike MAC addresses, IP addresses can change depending on the network.

These notes cover:

* What is an IP Address
* Why IP Addresses are Needed
* Logical vs Physical Address
* IPv4 Addressing
* IPv4 Classes (A, B, C, D, E)
* Types of IPv4 Addresses
* Public vs Private IP
* Static vs Dynamic IP
* Special IPv4 Addresses
* IPv6 Addressing
* IPv6 Address Types
* IPv4 vs IPv6 Comparison
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why IP Addresses are Needed](#2-why-ip-addresses-are-needed)
* [3. Logical vs Physical Address](#3-logical-vs-physical-address)
* [4. IPv4 Addressing](#4-ipv4-addressing)
* [5. IPv4 Address Classes](#5-ipv4-address-classes)
* [6. Types of IPv4 Addresses](#6-types-of-ipv4-addresses)
* [7. Public vs Private IP](#7-public-vs-private-ip)
* [8. Static vs Dynamic IP](#8-static-vs-dynamic-ip)
* [9. Special IPv4 Addresses](#9-special-ipv4-addresses)
* [10. IPv6 Addressing](#10-ipv6-addressing)
* [11. IPv6 Address Types](#11-ipv6-address-types)
* [12. IPv4 vs IPv6](#12-ipv4-vs-ipv6)
* [13. Cybersecurity Perspective](#13-cybersecurity-perspective)
* [14. Quick Revision Sheet](#14-quick-revision-sheet)

---

# 1. Introduction

IP stands for:

```text
Internet Protocol
```

An IP Address is a logical identifier assigned to a device connected to a network.

Examples:

```text
192.168.1.10
10.0.0.5
172.16.10.25
```

IPv6 Examples:

```text
2001:db8::1
fe80::1
```

Main purpose:

```text
Identify devices
and
enable communication
```

---

# 2. Why IP Addresses are Needed

Suppose millions of devices are connected to the Internet:

* Computers
* Smartphones
* Servers
* Routers
* IoT Devices

Each device must have a unique identifier.

Without IP addresses:

```text
Packets would not know
where to go.
```

Think of an IP address like:

```text
A postal address for a device
```

---

# 3. Logical vs Physical Address

| Address Type | Layer   | Example           |
| ------------ | ------- | ----------------- |
| MAC Address  | Layer 2 | 00:1A:2B:3C:4D:5E |
| IP Address   | Layer 3 | 192.168.1.10      |

---

## MAC Address

* Hardware address
* Assigned to NIC
* Used inside local network
* Usually permanent

---

## IP Address

* Logical address
* Can change
* Assigned manually or dynamically
* Used across networks

---

## Easy Memory Trick

```text
MAC = Who you are

IP = Where you are
```

---

# 4. IPv4 Addressing

IPv4 stands for:

```text
Internet Protocol Version 4
```

IPv4 address size:

```text
32 Bits
```

Divided into:

```text
4 Octets
```

Example:

```text
192.168.1.10
```

Each octet contains:

```text
8 Bits
```

Range:

```text
0 - 255
```

---

## IPv4 Structure

```text
192 . 168 . 1 . 10
 8     8     8    8

Total = 32 Bits
```

---

## Total IPv4 Addresses

Formula:

```text
2^32
```

Result:

```text
4,294,967,296
```

Approximately:

```text
4.3 Billion Addresses
```

---

# 5. IPv4 Address Classes

Before CIDR and subnetting became common, IPv4 addresses were divided into classes.

Purpose:

```text
Support networks of different sizes
```

Classes:

```text
Class A
Class B
Class C
Class D
Class E
```

---

## Class A

First Octet Range:

```text
1 - 126
```

Bit Pattern:

```text
0xxxxxxx
```

Default Subnet Mask:

```text
255.0.0.0
```

CIDR Equivalent:

```text
/8
```

Structure:

```text
N.H.H.H
```

Example:

```text
10.1.1.1
```

Network Portion:

```text
10
```

Host Portion:

```text
1.1.1
```

Hosts Per Network:

```text
2^24 - 2
=
16,777,214
```

Used for:

* Large Enterprises
* Governments
* ISPs

---

## Class B

First Octet Range:

```text
128 - 191
```

Bit Pattern:

```text
10xxxxxx
```

Default Subnet Mask:

```text
255.255.0.0
```

CIDR Equivalent:

```text
/16
```

Structure:

```text
N.N.H.H
```

Example:

```text
172.16.10.5
```

Hosts Per Network:

```text
2^16 - 2
=
65,534
```

Used for:

* Universities
* Medium-sized Organizations

---

## Class C

First Octet Range:

```text
192 - 223
```

Bit Pattern:

```text
110xxxxx
```

Default Subnet Mask:

```text
255.255.255.0
```

CIDR Equivalent:

```text
/24
```

Structure:

```text
N.N.N.H
```

Example:

```text
192.168.1.10
```

Hosts Per Network:

```text
2^8 - 2
=
254
```

Used for:

* Home Networks
* Small Businesses

---

## Class D

Range:

```text
224 - 239
```

Bit Pattern:

```text
1110xxxx
```

Used for:

```text
Multicast Communication
```

Examples:

* IPTV
* Video Streaming
* Online Conferences

Important:

```text
Not assigned to hosts
```

---

## Class E

Range:

```text
240 - 255
```

Bit Pattern:

```text
1111xxxx
```

Used for:

```text
Research
Experimental Purposes
```

Important:

```text
Not used in normal communication
```

---

## Class Comparison Table

| Class | Range   | Default Mask  | CIDR | Hosts per Network |
| ----- | ------- | ------------- | ---- | ----------------- |
| A     | 1-126   | 255.0.0.0     | /8   | 16,777,214        |
| B     | 128-191 | 255.255.0.0   | /16  | 65,534            |
| C     | 192-223 | 255.255.255.0 | /24  | 254               |
| D     | 224-239 | Multicast     | N/A  | N/A               |
| E     | 240-255 | Experimental  | N/A  | N/A               |

---

## Why Classful Addressing Was Replaced

Problem:

Many organizations received more IP addresses than needed.

Example:

```text
Need: 500 Hosts
```

Class C:

```text
254 Hosts
```

Too small.

Class B:

```text
65,534 Hosts
```

Too large.

Result:

```text
IPv4 Address Waste
```

Modern networks use:

```text
CIDR
(Classless Inter-Domain Routing)
```

which solves this problem.

---

## Memory Trick

```text
A = Very Large Networks

B = Medium Networks

C = Small Networks

D = Multicast

E = Experimental
```

---

# 6. Types of IPv4 Addresses

## Unicast

Communication between:

```text
One Sender
↓
One Receiver
```

Most Internet communication uses unicast.

---

## Broadcast

Communication to:

```text
All Devices
in a Network
```

Example:

```text
255.255.255.255
```

---

## Multicast

Communication to:

```text
Group of Devices
```

Examples:

* IPTV
* Video Streaming
* Live Broadcasts

---

# 7. Public vs Private IP

## Public IP

Used on:

```text
Internet
```

Globally unique.

Example:

```text
8.8.8.8
```

Assigned by:

```text
ISP
```

---

## Private IP

Used inside:

```text
Internal Networks
```

---

### Private Class A

```text
10.0.0.0 - 10.255.255.255
```

---

### Private Class B

```text
172.16.0.0 - 172.31.255.255
```

---

### Private Class C

```text
192.168.0.0 - 192.168.255.255
```

---

## Why Private IPs Exist

To conserve public IPv4 addresses.

Example:

```text
100 Devices
↓
One Router
↓
One Public IP
```

---

# 8. Static vs Dynamic IP

## Static IP

Characteristics:

* Fixed
* Does not change
* Commonly used by servers

Examples:

* Web Servers
* Database Servers

---

## Dynamic IP

Assigned automatically.

Usually by:

```text
DHCP
```

Characteristics:

* Automatic
* Changes over time
* Common for end users

---

# 9. Special IPv4 Addresses

## Loopback

Range:

```text
127.0.0.0/8
```

Most common:

```text
127.0.0.1
```

Meaning:

```text
This Computer
```

---

## APIPA

Range:

```text
169.254.0.0/16
```

Assigned when DHCP server is unavailable.

---

## Network Address

Represents:

```text
Entire Network
```

Example:

```text
192.168.1.0
```

---

## Broadcast Address

Represents:

```text
All Hosts
```

Example:

```text
192.168.1.255
```

---

## Default Route

```text
0.0.0.0
```

Meaning:

```text
Unknown Destination
```

Send to default gateway.

---

# 10. IPv6 Addressing

IPv6 stands for:

```text
Internet Protocol Version 6
```

Created because IPv4 addresses became insufficient.

---

## IPv6 Size

```text
128 Bits
```

Compared to:

```text
IPv4 = 32 Bits
```

---

## Example

```text
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

---

## Address Format

IPv6 uses:

```text
Hexadecimal
```

Contains:

```text
8 Groups
```

Each group:

```text
16 Bits
```

---

## Total IPv6 Addresses

Formula:

```text
2^128
```

Result:

```text
Approximately 340 Undecillion Addresses
```

---

## IPv6 Compression Rules

### Rule 1

Remove leading zeros.

Example:

```text
2001:0db8:0000:0001
```

becomes:

```text
2001:db8:0:1
```

---

### Rule 2

Replace consecutive zeros once with:

```text
::
```

Example:

```text
2001:db8:0:0:0:0:0:1
```

becomes:

```text
2001:db8::1
```

---

# 11. IPv6 Address Types

## Global Unicast

Equivalent to:

```text
Public IPv4
```

Used on Internet.

---

## Link Local

Range:

```text
fe80::/10
```

Used only inside local network.

---

## Unique Local Address

Equivalent to:

```text
Private IPv4
```

Range:

```text
fc00::/7
```

---

## Multicast

Prefix:

```text
ff00::/8
```

---

## Anycast

Traffic goes to:

```text
Nearest Device
```

Commonly used by:

* CDNs
* DNS Providers

---

## Important Difference

IPv6 does NOT use:

```text
Broadcast
```

Instead it uses:

```text
Multicast
```

---

# 12. IPv4 vs IPv6

| Feature       | IPv4      | IPv6                 |
| ------------- | --------- | -------------------- |
| Size          | 32 Bit    | 128 Bit              |
| Format        | Decimal   | Hexadecimal          |
| Broadcast     | Supported | Not Used             |
| NAT           | Common    | Usually Not Required |
| Address Space | Limited   | Extremely Large      |

---

# 13. Cybersecurity Perspective

## Why Security Engineers Should Know IP Addressing

Used in:

* Log Analysis
* Threat Hunting
* Firewall Rules
* Network Reconnaissance
* SIEM Investigations
* Incident Response

---

## IP Spoofing

Attacker changes source IP.

Purpose:

* Hide identity
* Reflection attacks
* Bypass filtering

---

## Private IP Identification

Common private ranges:

```text
10.x.x.x

172.16-31.x.x

192.168.x.x
```

SOC analysts encounter these constantly.

---

## IPv6 Security Challenges

Many organizations secure:

```text
IPv4
```

but ignore:

```text
IPv6
```

This may create:

* Blind spots
* Rogue IPv6 devices
* Security policy bypasses

---

# 14. Quick Revision Sheet

## Important Versions

```text
IPv4 = 32 Bit

IPv6 = 128 Bit
```

---

## Private IPv4 Ranges

```text
10.0.0.0/8

172.16.0.0/12

192.168.0.0/16
```

---

## Important Special Addresses

```text
127.0.0.1 → Loopback

169.254.0.0/16 → APIPA

255.255.255.255 → Broadcast

0.0.0.0 → Default Route
```

---

## IPv6 Important Prefixes

```text
2000::/3 → Global Unicast

fe80::/10 → Link Local

fc00::/7 → Unique Local

ff00::/8 → Multicast
```

---

## Class Memory Trick

```text
A → Very Large Networks

B → Medium Networks

C → Small Networks

D → Multicast

E → Experimental
```

---

## Biggest Concept

```text
MAC Address tells
who the device is locally.

IP Address tells
where the device is located
in a network.
```

---

*End of IP Addressing Notes*
