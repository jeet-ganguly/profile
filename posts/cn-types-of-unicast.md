# Computer Networks — Types of Unicast Addresses in IPv6

> Unicast communication means one sender communicates with exactly one receiver.
>
> IPv6 supports several types of unicast addresses designed for Internet communication, local communication, private networks, and compatibility with IPv4.

These notes cover:

* What is a Unicast Address
* Types of IPv6 Unicast Addresses
* Global Unicast Address
* Link-Local Address
* Unique Local Address
* Loopback Address
* Unspecified Address
* IPv4-Mapped Address
* Special-Purpose Addresses
* Comparison of Unicast Address Types
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. What is Unicast Communication](#2-what-is-unicast-communication)
* [3. Types of IPv6 Unicast Addresses](#3-types-of-ipv6-unicast-addresses)
* [4. Global Unicast Address](#4-global-unicast-address)
* [5. Link-Local Address](#5-link-local-address)
* [6. Unique Local Address](#6-unique-local-address)
* [7. Loopback Address](#7-loopback-address)
* [8. Unspecified Address](#8-unspecified-address)
* [9. IPv4-Mapped Address](#9-ipv4-mapped-address)
* [10. Comparison Table](#10-comparison-table)
* [11. Cybersecurity Perspective](#11-cybersecurity-perspective)
* [12. Quick Revision Sheet](#12-quick-revision-sheet)

---

# 1. Introduction

Unicast communication means:

```text
One Sender
      ↓
One Receiver
```

Most Internet traffic uses unicast communication.

Examples:

* Web Browsing
* SSH
* Email
* FTP
* DNS Queries

---

# 2. What is Unicast Communication

In unicast communication:

```text
Device A
     ↓
Device B
```

only one destination receives the packet.

Example:

```text
Laptop
   ↓
Web Server
```

Unlike multicast:

```text
One Sender
↓
Multiple Receivers
```

Unicast communication targets a single host.

---

# 3. Types of IPv6 Unicast Addresses

IPv6 defines the following unicast addresses:

```text
Global Unicast Address

Link-Local Address

Unique Local Address

Loopback Address

Unspecified Address

IPv4-Mapped Address
```

---

# 4. Global Unicast Address

Equivalent to:

```text
Public IPv4 Address
```

Used for:

```text
Internet Communication
```

---

## Prefix

```text
2000::/3
```

---

## Characteristics

* Globally Unique
* Internet Routable
* Assigned by ISP
* Publicly Accessible

---

## Example

```text
2001:db8:abcd:1234::1
```

---

## Applications

* Websites
* Cloud Servers
* Email Servers
* Internet Services

---

## Similar IPv4 Address

```text
8.8.8.8
```

---

# 5. Link-Local Address

Equivalent to:

```text
APIPA Address
```

in IPv4.

---

## Prefix

```text
fe80::/10
```

---

## Characteristics

* Automatically Assigned
* Exists on Every IPv6 Interface
* Used only within local network
* Not routable on Internet

---

## Example

```text
fe80::20c:29ff:fe9c:409
```

---

## Uses

* Neighbor Discovery
* Router Discovery
* Local Communication

---

## Important

Packets containing Link-Local addresses:

```text
Cannot Pass Through Routers
```

---

# 6. Unique Local Address

Equivalent to:

```text
Private IPv4 Address
```

---

## Prefix

```text
fc00::/7
```

Usually:

```text
fd00::/8
```

is used.

---

## Characteristics

* Private Network Address
* Internal Communication
* Not Internet Routable

---

## Example

```text
fd12:3456:789a::1
```

---

## Applications

* Enterprise Networks
* Data Centers
* Internal Servers

---

## Similar IPv4 Addresses

```text
10.0.0.0/8

172.16.0.0/12

192.168.0.0/16
```

---

# 7. Loopback Address

Equivalent to:

```text
127.0.0.1
```

in IPv4.

---

## Address

```text
::1
```

---

## Meaning

```text
This Computer
```

---

## Uses

* Testing
* Troubleshooting
* Local Services

---

## Example

Pinging loopback:

```bash
ping6 ::1
```

---

# 8. Unspecified Address

Equivalent to:

```text
0.0.0.0
```

in IPv4.

---

## Address

```text
::
```

---

## Meaning

```text
No Address Assigned
```

---

## Applications

Used during:

* Address Configuration
* DHCPv6
* Initialization

---

## Important

The unspecified address:

```text
Cannot Be Assigned
to an Interface
```

---

# 9. IPv4-Mapped Address

Used for:

```text
IPv4 Compatibility
```

---

## Format

```text
::ffff:x.x.x.x
```

---

## Example

```text
::ffff:192.168.1.10
```

---

## Purpose

Allows:

```text
IPv6 Applications
```

to communicate with:

```text
IPv4 Systems
```

---

## Applications

* Transition Mechanisms
* Dual Stack Systems
* Compatibility Support

---

# 10. Comparison Table

| Address Type   | Prefix         | IPv4 Equivalent | Internet Routable |
| -------------- | -------------- | --------------- | ----------------- |
| Global Unicast | 2000::/3       | Public IP       | Yes               |
| Link-Local     | fe80::/10      | APIPA           | No                |
| Unique Local   | fc00::/7       | Private IP      | No                |
| Loopback       | ::1            | 127.0.0.1       | No                |
| Unspecified    | ::             | 0.0.0.0         | No                |
| IPv4-Mapped    | ::ffff:x.x.x.x | IPv4 Address    | Depends           |

---

# 11. Cybersecurity Perspective

---

## Link-Local Address Abuse

Attackers may use:

```text
fe80::/10
```

for:

* Local Reconnaissance
* Lateral Movement

---

## Rogue Router Advertisements

Attackers can exploit:

* Neighbor Discovery Protocol
* Router Advertisements

to perform:

* MITM Attacks
* Traffic Redirection

---

## Unique Local Address Exposure

Accidental leakage of:

```text
fc00::/7
```

may reveal:

* Internal Topology
* Network Structure

---

## IPv6 Blind Spots

Many organizations secure:

```text
IPv4
```

but ignore:

```text
IPv6
```

leading to:

* Security Gaps
* Undetected Devices
* Policy Bypass

---

# 12. Quick Revision Sheet

Global Unicast:

```text
2000::/3
```

Equivalent:

```text
Public IPv4
```

---

Link-Local:

```text
fe80::/10
```

Equivalent:

```text
APIPA
```

---

Unique Local:

```text
fc00::/7
```

Equivalent:

```text
Private IPv4
```

---

Loopback:

```text
::1
```

Equivalent:

```text
127.0.0.1
```

---

Unspecified:

```text
::
```

Equivalent:

```text
0.0.0.0
```

---

IPv4-Mapped:

```text
::ffff:x.x.x.x
```

Used for:

```text
IPv4 Compatibility
```

---

## Biggest Concept

```text
IPv6 Unicast Addresses
provide one-to-one communication
and are designed for Internet,
local, private, and compatibility purposes.
```

---

*End of Types of IPv6 Unicast Addresses Notes*
