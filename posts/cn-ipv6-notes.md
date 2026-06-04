# Computer Networks — IPv6

> IPv6 (Internet Protocol Version 6) is the successor to IPv4.
>
> It was developed to solve the problem of IPv4 address exhaustion and to provide a larger address space, better scalability, and improved support for modern networks.

These notes cover:

* What is IPv6
* Why IPv6 was introduced
* IPv6 Address Structure
* IPv4 vs IPv6
* IPv6 Address Representation
* IPv6 Compression Rules
* IPv6 Address Types
* IPv6 Prefix Length
* IPv6 Interface ID
* Loopback Address
* IPv6 Special Addresses
* Advantages of IPv6
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why IPv6 was Introduced](#2-why-ipv6-was-introduced)
* [3. IPv6 Address Structure](#3-ipv6-address-structure)
* [4. IPv4 vs IPv6](#4-ipv4-vs-ipv6)
* [5. IPv6 Address Representation](#5-ipv6-address-representation)
* [6. IPv6 Compression Rules](#6-ipv6-compression-rules)
* [7. IPv6 Address Types](#7-ipv6-address-types)
* [8. Prefix Length and Interface ID](#8-prefix-length-and-interface-id)
* [9. Special IPv6 Addresses](#9-special-ipv6-addresses)
* [10. Advantages of IPv6](#10-advantages-of-ipv6)
* [11. Cybersecurity Perspective](#11-cybersecurity-perspective)
* [12. Quick Revision Sheet](#12-quick-revision-sheet)

---

# 1. Introduction

IPv6 stands for:

```text
Internet Protocol Version 6
```

IPv6 is the next generation Internet Protocol designed to replace IPv4.

Example:

```text
2001:db8:85a3::8a2e:370:7334
```

---

# 2. Why IPv6 was Introduced

IPv4 uses:

```text
32 Bits
```

Total addresses:

```text
2^32

≈ 4.3 Billion
```

Due to the rapid growth of:

* Smartphones
* IoT Devices
* Cloud Computing
* Servers

IPv4 addresses became insufficient.

IPv6 uses:

```text
128 Bits
```

Total addresses:

```text
2^128

≈ 340 Undecillion Addresses
```

---

# 3. IPv6 Address Structure

IPv6 address size:

```text
128 Bits
```

Divided into:

```text
8 Blocks
```

Each block contains:

```text
16 Bits
```

Example:

```text
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

---

## Total Number of Hex Digits

Each block contains:

```text
4 Hex Digits
```

Total:

```text
8 × 4 = 32 Hex Digits
```

---

# 4. IPv4 vs IPv6

| Feature        | IPv4      | IPv6                         |
| -------------- | --------- | ---------------------------- |
| Size           | 32 Bits   | 128 Bits                     |
| Address Space  | Limited   | Extremely Large              |
| Representation | Decimal   | Hexadecimal                  |
| Broadcast      | Supported | Not Used                     |
| NAT            | Common    | Usually Not Required         |
| Header Size    | Variable  | Fixed                        |
| Configuration  | DHCP      | Auto Configuration Supported |

---

# 5. IPv6 Address Representation

Example:

```text
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

Uses:

```text
Hexadecimal Numbers
```

Allowed characters:

```text
0-9

A-F
```

Blocks are separated by:

```text
:
```

---

# 6. IPv6 Compression Rules

Long IPv6 addresses can be shortened.

---

## Rule 1: Remove Leading Zeros

Example:

Original:

```text
2001:0db8:0000:0001
```

Compressed:

```text
2001:db8:0:1
```

---

## Rule 2: Consecutive Zeros can be replaced with ::

Example:

Original:

```text
2001:db8:0:0:0:0:0:1
```

Compressed:

```text
2001:db8::1
```

---

## Important Rule

```text
:: can be used only once
```

Otherwise the address becomes ambiguous.

---

# 7. IPv6 Address Types

IPv6 mainly uses:

* Global Unicast
* Link Local
* Unique Local
* Multicast
* Anycast

---

## Global Unicast Address

Equivalent to:

```text
Public IPv4 Address
```

Prefix:

```text
2000::/3
```

Used for Internet communication.

---

## Link Local Address

Prefix:

```text
fe80::/10
```

Characteristics:

* Automatically assigned
* Used inside local network
* Cannot be routed on Internet

---

## Unique Local Address (ULA)

Equivalent to:

```text
Private IPv4 Address
```

Prefix:

```text
fc00::/7
```

Used inside organizations.

---

## Multicast Address

Prefix:

```text
ff00::/8
```

One sender communicates with multiple receivers.

Examples:

* Video Streaming
* IPTV
* Routing Protocols

---

## Anycast Address

Same address assigned to multiple devices.

Traffic goes to:

```text
Nearest Device
```

Commonly used by:

* CDNs
* DNS Servers

---

## Important Difference

IPv6 does NOT use:

```text
Broadcast Address
```

Instead it uses:

```text
Multicast
```

---

# 8. Prefix Length and Interface ID

Similar to Network ID and Host ID in IPv4.

Typical IPv6 address:

```text
2001:db8:abcd:1234::1/64
```

Network Prefix:

```text
First 64 Bits
```

Interface ID:

```text
Last 64 Bits
```

Thus:

```text
64 Bits + 64 Bits

= 128 Bits
```

---

## Why /64 is Common

Most IPv6 networks use:

```text
/64
```

because:

* Stateless Auto Configuration works efficiently.
* Standard practice in IPv6 deployment.

---

# 9. Special IPv6 Addresses

---

## Loopback Address

Equivalent to:

```text
127.0.0.1
```

Address:

```text
::1
```

Represents:

```text
This Computer
```

---

## Unspecified Address

Address:

```text
::
```

Equivalent to:

```text
0.0.0.0
```

Represents:

```text
No Address Assigned
```

---

## IPv4-Mapped Address

Format:

```text
::ffff:x.x.x.x
```

Example:

```text
::ffff:192.168.1.10
```

Used for IPv4 compatibility.

---

# 10. Advantages of IPv6

Advantages:

* Huge Address Space
* Better Scalability
* Auto Configuration
* Simplified Header
* No Need for NAT
* Efficient Routing
* Better Multicast Support
* Improved Mobility Support

---

# 11. Cybersecurity Perspective

Security professionals should understand IPv6 because:

* Modern operating systems enable IPv6 by default.
* Attackers may exploit unmonitored IPv6 traffic.
* IPv6 can bypass IPv4-only firewall rules.

---

## Rogue Router Advertisements

Attackers may send fake Router Advertisements to:

* Redirect traffic
* Perform Man-in-the-Middle attacks

---

## IPv6 Blind Spots

Organizations often secure:

```text
IPv4
```

but ignore:

```text
IPv6
```

leading to:

* Security gaps
* Undetected devices
* Bypassed policies

---

## Reconnaissance

Attackers collect:

* IPv6 prefixes
* DNS records
* Network topology information

during the reconnaissance phase.

---

# 12. Quick Revision Sheet

IPv4:

```text
32 Bits
```

IPv6:

```text
128 Bits
```

---

Loopback:

```text
::1
```

---

Unspecified Address:

```text
::
```

---

Global Unicast:

```text
2000::/3
```

---

Link Local:

```text
fe80::/10
```

---

Unique Local:

```text
fc00::/7
```

---

Multicast:

```text
ff00::/8
```

---

Most Common Prefix:

```text
/64
```

---

Most Important Difference:

```text
IPv6 does not use Broadcast.

It uses Multicast instead.
```

---

Biggest Concept:

```text
IPv6 solves the address exhaustion problem of IPv4 and provides an enormous address space for the future Internet.
```

---

*End of IPv6 Notes*
