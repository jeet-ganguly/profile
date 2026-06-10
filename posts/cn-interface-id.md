# Computer Networks — Interface ID Assignment in IPv6

> An IPv6 address is 128 bits long and is generally divided into two parts:
>
> * Network Prefix
> * Interface Identifier (Interface ID)
>
> The Interface ID uniquely identifies an interface (host/device) within a network. IPv6 provides several methods to assign the Interface ID automatically or manually.

These notes cover:

* What is Interface ID
* IPv6 Address Structure
* Need for Interface ID
* Methods of Interface ID Assignment
* EUI-64 Method
* Modified EUI-64
* Random Interface ID
* DHCPv6 Assignment
* Manual Assignment
* Privacy Extensions
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. IPv6 Address Structure](#2-ipv6-address-structure)
* [3. What is Interface ID](#3-what-is-interface-id)
* [4. Why Interface ID is Needed](#4-why-interface-id-is-needed)
* [5. Methods of Interface ID Assignment](#5-methods-of-interface-id-assignment)
* [6. EUI-64 Method](#6-eui-64-method)
* [7. Modified EUI-64 Process](#7-modified-eui-64-process)
* [8. Random Interface ID (Privacy Extension)](#8-random-interface-id-privacy-extension)
* [9. DHCPv6 Assignment](#9-dhcpv6-assignment)
* [10. Manual Assignment](#10-manual-assignment)
* [11. Comparison of Methods](#11-comparison-of-methods)
* [12. Cybersecurity Perspective](#12-cybersecurity-perspective)
* [13. Quick Revision Sheet](#13-quick-revision-sheet)

---

# 1. Introduction

A typical IPv6 address contains:

```text
Network Prefix
+
Interface ID
```

Example:

```text
2001:db8:abcd:1234:021c:7eff:fe10:4a5b
```

Here:

```text
2001:db8:abcd:1234
```

represents the:

```text
Network Prefix
```

and

```text
021c:7eff:fe10:4a5b
```

represents the:

```text
Interface ID
```

---

# 2. IPv6 Address Structure

IPv6 size:

```text
128 Bits
```

Usually divided as:

```text
64 Bits Network Prefix
+
64 Bits Interface ID
```

Example:

```text
2001:db8:abcd:1234 : 021c:7eff:fe10:4a5b
```

Therefore:

```text
First 64 Bits
```

identify the network and

```text
Last 64 Bits
```

identify the host interface.

---

# 3. What is Interface ID

Interface ID is used to uniquely identify a device inside a network.

Think of:

```text
Network Prefix
```

as:

```text
Street Name
```

and

```text
Interface ID
```

as:

```text
House Number
```

Thus:

```text
Same Network Prefix
Different Interface IDs
```

allow multiple devices to exist on the same network.

---

# 4. Why Interface ID is Needed

Without Interface IDs:

```text
All devices would have
the same IPv6 address.
```

Interface IDs help:

* Identify devices uniquely.
* Enable communication.
* Support automatic configuration.
* Simplify network management.

---

# 5. Methods of Interface ID Assignment

IPv6 supports several methods:

```text
EUI-64 Method

Random Interface ID

DHCPv6

Manual Assignment
```

---

# 6. EUI-64 Method

EUI means:

```text
Extended Unique Identifier
```

This method generates Interface ID from the:

```text
MAC Address
```

of the network card.

---

## Example MAC Address

```text
00:1C:7E:10:4A:5B
```

48 bits long.

IPv6 Interface ID requires:

```text
64 Bits
```

Therefore, additional bits are inserted.

---

# 7. Modified EUI-64 Process

Suppose the MAC address is:

```text
00:1C:7E:10:4A:5B
```

---

## Step 1

Split MAC into two parts:

```text
00:1C:7E

10:4A:5B
```

---

## Step 2

Insert:

```text
FF:FE
```

between them.

Result:

```text
00:1C:7E:FF:FE:10:4A:5B
```

Now length becomes:

```text
64 Bits
```

---

## Step 3

Flip the Universal/Local bit.

Final Interface ID becomes:

```text
021C:7EFF:FE10:4A5B
```

---

## Complete IPv6 Address

Suppose prefix is:

```text
2001:db8:abcd:1234::/64
```

Final IPv6 address:

```text
2001:db8:abcd:1234:021c:7eff:fe10:4a5b
```

---

# Advantages of EUI-64

* Automatic
* No DHCP server needed
* Easy configuration

---

# Disadvantages

Since Interface ID depends on MAC address:

```text
Device Tracking
```

becomes possible.

This creates privacy concerns.

---

# 8. Random Interface ID (Privacy Extension)

Modern operating systems usually use:

```text
Random Interface IDs
```

instead of EUI-64.

Purpose:

```text
Improve Privacy
```

because Interface IDs no longer reveal:

```text
MAC Address
```

---

## Example

IPv6 Address:

```text
2001:db8:abcd:1234:7c9a:32bf:48ef:98a1
```

Here:

```text
7c9a:32bf:48ef:98a1
```

is randomly generated.

---

## Benefits

* Better Privacy
* Prevent Device Tracking
* More Secure

---

# 9. DHCPv6 Assignment

DHCPv6 stands for:

```text
Dynamic Host Configuration Protocol for IPv6
```

A DHCPv6 server assigns:

* IPv6 Address
* Interface ID
* DNS Information

similar to IPv4 DHCP.

---

## Advantages

* Centralized Management
* Easy Administration
* Suitable for Enterprises

---

## Applications

* Data Centers
* Corporate Networks
* ISP Networks

---

# 10. Manual Assignment

Administrator manually configures:

```text
Entire IPv6 Address
```

including Interface ID.

Example:

```text
2001:db8:abcd:1234::10
```

Here:

```text
10
```

acts as Interface ID.

---

## Applications

* Routers
* Servers
* Firewalls
* Critical Infrastructure

---

# 11. Comparison of Methods

| Method              | Generated By     | Advantages             |
| ------------------- | ---------------- | ---------------------- |
| EUI-64              | MAC Address      | Automatic              |
| Random Interface ID | Operating System | Better Privacy         |
| DHCPv6              | DHCP Server      | Centralized Management |
| Manual              | Administrator    | Full Control           |

---

# 12. Cybersecurity Perspective

---

## EUI-64 Information Leakage

Since EUI-64 uses:

```text
MAC Address
```

attackers may determine:

* Device Vendor
* Device Identity
* Device Tracking Information

---

## Privacy Extensions

Modern systems use:

```text
Random Interface IDs
```

to prevent:

* Tracking
* Profiling
* Information Leakage

---

## DHCPv6 Attacks

Attackers may deploy:

```text
Rogue DHCPv6 Servers
```

to:

* Redirect Traffic
* Provide Malicious DNS Servers

---

## Reconnaissance

During IPv6 enumeration, attackers often analyze:

* Interface IDs
* Prefixes
* Neighbor Discovery Messages

to identify devices.

---

# 13. Quick Revision Sheet

IPv6 Address:

```text
64 Bits Network Prefix

+

64 Bits Interface ID
```

---

Interface ID Assignment Methods:

```text
EUI-64

Random Interface ID

DHCPv6

Manual Assignment
```

---

EUI-64 Uses:

```text
MAC Address
```

---

Privacy Extensions Use:

```text
Random Numbers
```

---

DHCPv6 Provides:

```text
Centralized Assignment
```

---

Manual Assignment Provides:

```text
Full Administrative Control
```

---

## Biggest Concept

```text
The Interface ID uniquely
identifies a host inside an IPv6 network
and can be assigned automatically,
randomly, manually, or through DHCPv6.
```

---

*End of Interface ID Assignment in IPv6 Notes*
