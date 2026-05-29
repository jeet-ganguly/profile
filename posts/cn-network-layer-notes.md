# Computer Networks — Network Layer

> The Network Layer is responsible for delivering data between different networks.
>
> It handles:
>
> * Logical addressing
> * Routing
> * Path selection
> * Packet forwarding
> * Inter-network communication
>
> The Network Layer enables communication across the Internet by determining how packets travel from source to destination.

These notes cover:

* What is Network Layer
* Why Network Layer exists
* Logical addressing
* IP addressing concepts
* Routing
* Routers
* Packet forwarding
* Routing tables
* Static and Dynamic Routing
* Packet switching
* Fragmentation
* TTL
* ICMP
* NAT basics
* Encapsulation at Layer 3
* Network Layer cyber attacks

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why Network Layer Exists](#2-why-network-layer-exists)
* [3. Main Responsibilities](#3-main-responsibilities)
* [4. Logical Addressing](#4-logical-addressing)
* [5. IP Address Basics](#5-ip-address-basics)
* [6. Routing](#6-routing)
* [7. Routers](#7-routers)
* [8. Routing Table](#8-routing-table)
* [9. Static vs Dynamic Routing](#9-static-vs-dynamic-routing)
* [10. Packet Forwarding](#10-packet-forwarding)
* [11. Packet Switching](#11-packet-switching)
* [12. Fragmentation](#12-fragmentation)
* [13. TTL (Time To Live)](#13-ttl-time-to-live)
* [14. ICMP Basics](#14-icmp-basics)
* [15. NAT Basics](#15-nat-basics)
* [16. Encapsulation at Network Layer](#16-encapsulation-at-network-layer)
* [17. Network Layer Cyber Attacks](#17-network-layer-cyber-attacks)
* [18. Quick Revision Sheet](#18-quick-revision-sheet)

---

# 1. Introduction

The Network Layer is:

```text
Layer 3 of OSI Model
```

It sits between:

```text
Transport Layer
↓
Network Layer
↓
Data Link Layer
```

Main purpose:

```text
Deliver packets between networks
```

This layer mainly works with:

```text
Packets
IP Addresses
Routers
```

---

# 2. Why Network Layer Exists

Suppose:

```text
Laptop in India
↓
Internet
↓
Server in USA
```

Questions arise:

* How does data find destination?
* Which route should be taken?
* What happens if multiple paths exist?
* How do routers know where to send packets?

The Network Layer solves these problems.

---

# Important Understanding

Data Link Layer handles:

```text
Local communication
```

Network Layer handles:

```text
Global communication
```

---

# 3. Main Responsibilities

| Responsibility              | Purpose                    |
| --------------------------- | -------------------------- |
| Logical Addressing          | Identify devices           |
| Routing                     | Select path                |
| Packet Forwarding           | Move packets               |
| Fragmentation               | Handle packet size issues  |
| Inter-network Communication | Connect different networks |

---

# 4. Logical Addressing

Every device connected to a network requires an address.

The Network Layer uses:

```text
IP Addresses
```

to identify devices.

Think of an IP address as:

```text
Home Address of a device
```

Without IP addressing:

```text
Packet would not know where to go
```

---

# Physical Address vs Logical Address

| Type        | Layer   | Example           |
| ----------- | ------- | ----------------- |
| MAC Address | Layer 2 | 00:1A:2B:3C:4D:5E |
| IP Address  | Layer 3 | 192.168.1.10      |

---

# Important Understanding

```text
MAC → Local Identity

IP → Global Identity
```

---

# 5. IP Address Basics

An IP address uniquely identifies a device.

Examples:

```text
192.168.1.10
10.0.0.5
172.16.0.20
```

---

# Why IP Addresses Are Needed

Suppose:

```text
Millions of devices
connected to Internet
```

Each device must have a unique identity.

Otherwise:

```text
Packets cannot reach destination
```

---

# IPv4 vs IPv6

| Feature       | IPv4        | IPv6        |
| ------------- | ----------- | ----------- |
| Size          | 32-bit      | 128-bit     |
| Address Space | Limited     | Huge        |
| Example       | 192.168.1.1 | 2001:db8::1 |

---

# Important Understanding

Network Layer does NOT care about:

```text
Web pages
Files
Messages
```

It only cares about:

```text
Source IP
Destination IP
```

---

# 6. Routing

Routing means:

```text
Selecting path
from source
to destination
```

---

# Example

```text
Computer
   ↓
Router A
   ↓
Router B
   ↓
Router C
   ↓
Destination
```

The Network Layer decides:

```text
Which path should be used
```

---

# Why Routing Is Needed

Internet contains:

```text
Millions of networks
```

Packets need directions.

Routing provides those directions.

---

# Important Understanding

Routing is similar to:

```text
Google Maps
for packets
```

---

# 7. Routers

Router is a Layer-3 device.

Main purpose:

```text
Connect multiple networks
```

---

# Router Responsibilities

* Read destination IP
* Consult routing table
* Select next hop
* Forward packet

---

# Important Understanding

Switch forwards using:

```text
MAC Address
```

Router forwards using:

```text
IP Address
```

---

# 8. Routing Table

Routers maintain:

```text
Routing Table
```

A routing table contains:

```text
Destination Network
↓
Next Hop
↓
Outgoing Interface
```

---

# Simplified Example

| Destination | Next Hop    |
| ----------- | ----------- |
| 192.168.1.0 | Interface 1 |
| 10.0.0.0    | Interface 2 |
| Default     | ISP Router  |

---

# Why Routing Tables Exist

Without routing tables:

```text
Router cannot determine
where packet should go
```

---

# 9. Static vs Dynamic Routing

## Static Routing

Routes manually configured.

Advantages:

* Simple
* Predictable

Disadvantages:

* Difficult to manage at scale

---

## Dynamic Routing

Routes learned automatically.

Advantages:

* Adaptive
* Scalable

Disadvantages:

* More complex

---

# Important Understanding

Static:

```text
Administrator decides routes
```

Dynamic:

```text
Routers learn routes automatically
```

---

# 10. Packet Forwarding

Packet forwarding means:

```text
Moving packet
toward destination
```

---

# Forwarding Process

1. Packet arrives
2. Router reads destination IP
3. Routing table checked
4. Best route selected
5. Packet forwarded

---

# Important Understanding

Routing decides:

```text
Where packet should go
```

Forwarding performs:

```text
Actual movement
```

---

# 11. Packet Switching

The Internet uses:

```text
Packet Switching
```

---

# Traditional Idea

Entire message sent together.

Problem:

```text
Inefficient
```

---

# Packet Switching Idea

Data divided into:

```text
Small packets
```

Example:

```text
Message
↓
Packet1
Packet2
Packet3
Packet4
```

Each packet may travel:

```text
Different route
```

---

# Advantages

* Efficient bandwidth usage
* Better scalability
* Fault tolerance

---

# 12. Fragmentation

Different networks support different packet sizes.

Maximum size called:

```text
MTU
(Maximum Transmission Unit)
```

---

# Problem

Suppose:

```text
Packet = 3000 Bytes
```

Network supports:

```text
1500 Bytes
```

---

# Solution

Packet divided into:

```text
Fragment 1
Fragment 2
Fragment 3
```

This process is called:

```text
Fragmentation
```

---

# Why Needed

Allows packet traversal through networks with different MTU sizes.

---

# 13. TTL (Time To Live)

TTL prevents packets from looping forever.

Every packet contains:

```text
TTL Value
```

Example:

```text
TTL = 64
```

---

# How It Works

Each router:

```text
Decreases TTL by 1
```

Example:

```text
64
↓
63
↓
62
↓
61
```

---

# If TTL Becomes Zero

Router drops packet.

Reason:

```text
Prevent infinite routing loops
```

---

# Important Cybersecurity Relevance

TTL is heavily used by:

* traceroute
* network diagnostics
* threat hunting

---

# 14. ICMP Basics

ICMP means:

```text
Internet Control Message Protocol
```

Used for:

```text
Error reporting
Network diagnostics
```

---

# Important Understanding

ICMP does NOT transport user data.

Instead it carries:

```text
Control information
```

---

# Examples

Used by:

```text
ping
traceroute
```

---

# Common Functions

* Reachability testing
* Error reporting
* Route diagnostics

---

# 15. NAT Basics

NAT means:

```text
Network Address Translation
```

---

# Why NAT Exists

Public IPv4 addresses are limited.

NAT allows:

```text
Many private devices
↓
Share one public IP
```

---

# Example

```text
192.168.1.10
192.168.1.20
192.168.1.30
        ↓
Router NAT
        ↓
Single Public IP
```

---

# Advantages

* Conserves IPv4 addresses
* Hides internal network structure

---

# Cybersecurity Benefit

Provides:

```text
Basic obscurity
```

for internal devices.

---

# 16. Encapsulation at Network Layer

At Layer 3:

```text
Segment
↓
Packet
```

---

# Encapsulation Process

Transport Layer sends:

```text
Segment
```

Network Layer adds:

```text
IP Header
```

Result:

```text
Packet
```

---

# Packet Structure

```text
IP Header
+
Transport Data
```

---

# Receiver Side

Network Layer removes:

```text
IP Header
```

Then passes data upward.

---

# 17. Network Layer Cyber Attacks

---

# IP Spoofing

Attacker changes source IP.

Purpose:

* Hide identity
* Bypass filtering
* Launch attacks

---

# ICMP Tunneling

Data hidden inside ICMP packets.

Used by:

```text
Malware
APT Groups
C2 Channels
```

---

# Smurf Attack

Attacker abuses broadcast ICMP traffic.

Result:

```text
Amplification Attack
```

---

# BGP Hijacking

Attacker advertises fake routes.

Result:

```text
Traffic Redirection
Traffic Interception
```

---

# Routing Manipulation

Attackers influence routing decisions.

Impact:

```text
MITM
Traffic Monitoring
```

---

# IPv6 Rogue Router Advertisement

Fake router advertisements sent.

Result:

```text
Traffic interception
```

---

# Packet Fragmentation Evasion

Attackers split packets to evade:

```text
Firewalls
IDS
IPS
```

---

# 18. Quick Revision Sheet

```text
Layer 3 = Global communication
```

---

# Main Responsibilities

```text
Logical Addressing
Routing
Forwarding
Fragmentation
```

---

# Important Concepts

```text
Packet = Layer-3 PDU

IP Address = Device Identity

Router = Layer-3 Device
```

---

# Important Devices

```text
Router → Layer 3

Layer 3 Switch → Layer 3
```

---

# Key Features

```text
Routing Table

TTL

ICMP

NAT

Packet Switching
```

---

# Biggest Concept

```text
The Network Layer decides
where packets should go
across different networks.
```

---

*End of Network Layer Notes*
