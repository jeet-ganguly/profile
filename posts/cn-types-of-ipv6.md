# Computer Networks — Types of IPv6 Addresses

> IPv6 supports different types of addresses to enable one-to-one, one-to-many, and one-to-nearest communication.
>
> Unlike IPv4, IPv6 does not use broadcast addresses. Instead, it mainly relies on Unicast, Multicast, and Anycast addresses.

These notes cover:

* IPv6 Address Categories
* Unicast Address
* Multicast Address
* Anycast Address
* Types of Unicast Addresses
* Global Unicast Address
* Link-Local Address
* Unique Local Address
* Special Addresses
* IPv4 vs IPv6 Address Types
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. IPv6 Address Categories](#2-ipv6-address-categories)
* [3. Unicast Address](#3-unicast-address)
* [4. Multicast Address](#4-multicast-address)
* [5. Anycast Address](#5-anycast-address)
* [6. Types of Unicast Addresses](#6-types-of-unicast-addresses)
* [7. Global Unicast Address](#7-global-unicast-address)
* [8. Link-Local Address](#8-link-local-address)
* [9. Unique Local Address](#9-unique-local-address)
* [10. Special IPv6 Addresses](#10-special-ipv6-addresses)
* [11. IPv4 vs IPv6 Address Types](#11-ipv4-vs-ipv6-address-types)
* [12. Cybersecurity Perspective](#12-cybersecurity-perspective)
* [13. Quick Revision Sheet](#13-quick-revision-sheet)

---

# 1. Introduction

IPv6 mainly supports three address categories:

```text
Unicast

Multicast

Anycast
```

Unlike IPv4:

```text
Broadcast Address
```

is not used in IPv6.

Instead, IPv6 uses:

```text
Multicast
```

for one-to-many communication.

---

# 2. IPv6 Address Categories

### Unicast

Communication between:

```text
One Sender
↓
One Receiver
```

---

### Multicast

Communication between:

```text
One Sender
↓
Multiple Receivers
```

---

### Anycast

Communication between:

```text
One Sender
↓
Nearest Receiver
```

---

# 3. Unicast Address

Unicast means:

```text
One-to-One Communication
```

One sender communicates with exactly one destination.

Example:

```text
Laptop
↓
Web Server
```

Most Internet traffic uses unicast communication.

Examples:

```text
Web Browsing

SSH

Email

FTP
```

---

# 4. Multicast Address

Multicast means:

```text
One-to-Many Communication
```

One sender transmits data to multiple receivers simultaneously.

Example:

```text
Video Server
       ↓
-------------------
↓        ↓        ↓

PC1     PC2      PC3
```

---

## Prefix

All multicast addresses begin with:

```text
ff00::/8
```

---

## Applications

* IPTV
* Video Conferencing
* Routing Protocols
* Streaming Services

---

## Why Multicast?

Without multicast:

```text
1 Sender
↓
Separate Copy for Every Device
```

With multicast:

```text
1 Sender
↓
One Stream
↓
Multiple Receivers
```

This saves:

* Bandwidth
* CPU Resources

---

# 5. Anycast Address

Anycast means:

```text
One-to-Nearest Communication
```

Multiple devices share the same address.

Traffic is delivered to:

```text
Nearest Device
```

according to routing metrics.

Example:

```text
User
 ↓

DNS Server A
(New York)

DNS Server B
(London)

DNS Server C
(Tokyo)
```

The packet reaches the closest server.

---

## Applications

* CDN Networks
* DNS Servers
* Load Balancing
* Cloud Services

---

## Example

Google DNS:

```text
8.8.8.8
```

and many CDN providers internally use Anycast to serve requests from the nearest location.

---

# 6. Types of Unicast Addresses

IPv6 Unicast addresses are further divided into:

```text
Global Unicast Address

Link-Local Address

Unique Local Address
```

---

# 7. Global Unicast Address

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
* Publicly Accessible

---

## Example

```text
2001:db8:abcd::1
```

---

## Uses

* Websites
* Cloud Servers
* Public Networks

---

# 8. Link-Local Address

Equivalent to:

```text
APIPA
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
* Used Inside Local Network
* Not Routable on Internet
* Every IPv6 Device Has One

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
Cannot Cross Routers
```

---

# 9. Unique Local Address

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

* Private Address
* Internal Networks
* Not Internet Routable

---

## Example

```text
fd12:3456:789a::1
```

---

## Uses

* Enterprise Networks
* Internal Servers
* Data Centers

---

# Comparison of Unicast Address Types

| Address Type   | Prefix    | Equivalent IPv4 |
| -------------- | --------- | --------------- |
| Global Unicast | 2000::/3  | Public IP       |
| Link Local     | fe80::/10 | APIPA           |
| Unique Local   | fc00::/7  | Private IP      |

---

# 10. Special IPv6 Addresses

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

Meaning:

```text
This Computer
```

---

## Unspecified Address

Equivalent to:

```text
0.0.0.0
```

Address:

```text
::
```

Meaning:

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

Used for:

```text
IPv4 Compatibility
```

---

# 11. IPv4 vs IPv6 Address Types

| Feature   | IPv4    | IPv6           |
| --------- | ------- | -------------- |
| Unicast   | Yes     | Yes            |
| Multicast | Yes     | Yes            |
| Anycast   | Limited | Native Support |
| Broadcast | Yes     | No             |

---

# Why IPv6 Does Not Use Broadcast

Broadcast packets consume:

* Bandwidth
* CPU Resources

IPv6 replaces broadcast with:

```text
Multicast
```

which is more efficient.

---

# 12. Cybersecurity Perspective

---

## Link-Local Address Abuse

Attackers may exploit:

```text
fe80::/10
```

for:

* Lateral Movement
* Local Reconnaissance

---

## Rogue Router Advertisement

Attackers may advertise fake routers and perform:

* MITM Attacks
* Traffic Redirection

---

## Anycast Benefits

Anycast provides:

* High Availability
* DDoS Resistance
* Better Load Distribution

---

## IPv6 Blind Spots

Many organizations secure:

```text
IPv4
```

but forget:

```text
IPv6
```

creating:

* Security Gaps
* Undetected Devices
* Policy Bypass

---

# 13. Quick Revision Sheet

IPv6 Address Categories:

```text
Unicast

Multicast

Anycast
```

---

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

Multicast:

```text
ff00::/8
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

Biggest Concept:

```text
IPv6 uses

Unicast
Multicast
Anycast

and completely eliminates
Broadcast communication.
```

---

*End of Types of IPv6 Addresses Notes*
