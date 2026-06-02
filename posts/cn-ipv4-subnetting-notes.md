# Computer Networks — IPv4 Subnetting

> Subnetting is the process of dividing a large network into smaller logical networks called subnets.
>
> It helps improve network organization, performance, security, and efficient utilization of IPv4 addresses.

These notes cover:

* What is Subnetting
* Why Subnetting is Needed
* Network ID and Host ID
* CIDR Notation
* Subnet Masks
* Borrowing Bits
* Host Calculation
* Network Calculation
* Practical Numerical Problems
* Subnet Ranges
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why Subnetting is Needed](#2-why-subnetting-is-needed)
* [3. Network ID and Host ID](#3-network-id-and-host-id)
* [4. CIDR Notation](#4-cidr-notation)
* [5. Subnet Mask Basics](#5-subnet-mask-basics)
* [6. Borrowing Bits](#6-borrowing-bits)
* [7. Host and Network Calculation](#7-host-and-network-calculation)
* [8. Practical Numerical Example](#8-practical-numerical-example)
* [9. Common CIDR Values](#9-common-cidr-values)
* [10. Cybersecurity Perspective](#10-cybersecurity-perspective)
* [11. Quick Revision Sheet](#11-quick-revision-sheet)

---

# 1. Introduction

Subnetting means:

```text id="sub1"
One Network
      ↓
Multiple Smaller Networks
```

Example:

```text id="sub2"
192.168.1.0/24
```

can be divided into:

```text id="sub3"
192.168.1.0/26

192.168.1.64/26

192.168.1.128/26

192.168.1.192/26
```

---

# 2. Why Subnetting is Needed

Without subnetting:

* Large broadcast domains
* Difficult management
* Poor network segmentation
* Difficult security control

Subnetting provides:

* Better organization
* Easier management
* Improved performance
* Better security isolation

---

# 3. Network ID and Host ID

Every IPv4 address contains:

```text id="sub4"
Network Portion
+
Host Portion
```

Example:

```text id="sub5"
192.168.1.10/24
```

Network Portion:

```text id="sub6"
192.168.1
```

Host Portion:

```text id="sub7"
10
```

---

# 4. CIDR Notation

CIDR means:

```text id="sub8"
Classless Inter-Domain Routing
```

Format:

```text id="sub9"
IP Address / Prefix
```

Example:

```text id="sub10"
192.168.1.0/24
```

Meaning:

```text id="sub11"
24 Network Bits

8 Host Bits
```

because:

```text id="sub12"
32 - 24 = 8
```

---

# 5. Subnet Mask Basics

Subnet mask separates:

```text id="sub13"
Network Portion
```

from:

```text id="sub14"
Host Portion
```

---

| CIDR | Subnet Mask     |
| ---- | --------------- |
| /24  | 255.255.255.0   |
| /25  | 255.255.255.128 |
| /26  | 255.255.255.192 |
| /27  | 255.255.255.224 |
| /28  | 255.255.255.240 |
| /29  | 255.255.255.248 |
| /30  | 255.255.255.252 |

---

# 6. Borrowing Bits

Subnetting works by borrowing host bits.

Example:

Original:

```text id="sub15"
192.168.1.0/24
```

New:

```text id="sub16"
192.168.1.0/26
```

Borrowed:

```text id="sub17"
26 - 24

= 2 Bits
```

---

# Important Rule

```text id="sub18"
More Borrowed Bits
       ↓
More Networks
       ↓
Less Hosts
```

and

```text id="sub19"
Less Borrowed Bits
       ↓
Less Networks
       ↓
More Hosts
```

---

# 7. Host and Network Calculation

Host Formula:

```text id="sub20"
Hosts = 2^(Host Bits) - 2
```

Network Formula:

```text id="sub21"
Networks = 2^(Borrowed Bits)
```

---

# Example

Network:

```text id="sub22"
192.168.1.0/26
```

Host Bits:

```text id="sub23"
32 - 26

= 6
```

Hosts:

```text id="sub24"
2^6 - 2

= 62
```

Borrowed Bits:

```text id="sub25"
26 - 24

= 2
```

Networks:

```text id="sub26"
2^2

= 4
```

---

# 8. Practical Numerical Example

Suppose a company owns:

```text id="sub27"
192.168.1.0/23 
```
Here we use class-less addressing that's why /23 , if we use /24 then 5 subnets not possible

Requirement:

```text id="sub28"
Need 5 Subnets

Each Subnet Must Support
60 Hosts
```

---

## Step 1: Find Required Host Bits

Formula:

```text id="sub29"
2^(Host Bits) - 2
```

Try:

```text id="sub30"
2^5 - 2

= 30
```

Not enough.

Try:

```text id="sub31"
2^6 - 2

= 62
```

Valid.

Therefore:

```text id="sub32"
Host Bits = 6
```

---

## Step 2: Find Prefix Length

```text id="sub33"
32 - 6

= 26
```

Required Prefix:

```text id="sub34"
/26
```

Subnet Mask:

```text id="sub35"
255.255.255.192
```

---

## Step 3: Verify Number of Subnets

Original:

```text id="sub36"
/23
```

New:

```text id="sub37"
/26
```

Borrowed:

```text id="sub38"
26 - 23

= 3 Bits
```

Networks:

```text id="sub39"
2^3

= 8
```

Available Subnets:

```text id="sub40"
8
```

Required:

```text id="sub41"
5
```

Requirement satisfied.

---

## Subnet 1

Network:

```text id="sub42"
192.168.0.0/26
```

Usable Hosts:

```text id="sub43"
192.168.0.1

to

192.168.0.62
```

Broadcast:

```text id="sub44"
192.168.0.63
```

---

## Subnet 2

Network:

```text id="sub45"
192.168.0.64/26
```

Usable Hosts:

```text id="sub46"
192.168.0.65

to

192.168.0.126
```

Broadcast:

```text id="sub47"
192.168.0.127
```

---

## Subnet 3

Network:

```text id="sub48"
192.168.0.128/26
```

Usable Hosts:

```text id="sub49"
192.168.0.129

to

192.168.0.190
```

Broadcast:

```text id="sub50"
192.168.0.191
```

---

## Subnet 4

Network:

```text id="sub51"
192.168.0.192/26
```

Usable Hosts:

```text id="sub52"
192.168.0.193

to

192.168.0.254
```

Broadcast:

```text id="sub53"
192.168.0.255
```

---

## Subnet 5

Network:

```text id="sub54"
192.168.1.0/26
```

Usable Hosts:

```text id="sub55"
192.168.1.1

to

192.168.1.62
```

Broadcast:

```text id="sub56"
192.168.1.63
```

---

## Final Answer

Chosen Prefix:

```text id="sub57"
/26
```

Subnet Mask:

```text id="sub58"
255.255.255.192
```

Hosts Per Subnet:

```text id="sub59"
62
```

Available Subnets:

```text id="sub60"
8
```

Requirement:

```text id="sub61"
5 Subnets

60 Hosts Each
```

Result:

```text id="sub62"
Requirement Satisfied
```

---

# 9. Common CIDR Values

| CIDR | Hosts |
| ---- | ----- |
| /24  | 254   |
| /25  | 126   |
| /26  | 62    |
| /27  | 30    |
| /28  | 14    |
| /29  | 6     |
| /30  | 2     |

---

# 10. Cybersecurity Perspective

Subnetting is widely used for:

* VLAN Segmentation
* Network Isolation
* Firewall Design
* Active Directory Networks
* Data Centers
* Cloud VPCs

Example:

```text id="sub63"
HR      → 192.168.10.0/24

Finance → 192.168.20.0/24

SOC     → 192.168.30.0/24
```

Benefits:

* Reduce attack surface
* Prevent lateral movement
* Easier monitoring
* Better access control

---

# 11. Quick Revision Sheet

Host Formula:

```text id="sub64"
Hosts = 2^(Host Bits) - 2
```

---

Network Formula:

```text id="sub65"
Networks = 2^(Borrowed Bits)
```

---

CIDR Formula:

```text id="sub66"
Host Bits

= 32 - Prefix Length
```

---

Most Common Values:

```text id="sub67"
/24 = 254 Hosts

/25 = 126 Hosts

/26 = 62 Hosts

/27 = 30 Hosts

/28 = 14 Hosts
```

---

Most Important Concept:

```text id="sub68"
More Networks
=
Less Hosts

More Hosts
=
Less Networks
```

---

*End of IPv4 Subnetting Notes*
