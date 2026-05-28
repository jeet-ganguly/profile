# Computer Networks — Data Link Layer

> The Data Link Layer is responsible for communication inside a local network.
>
> It handles:
>
> * Local delivery
> * Framing
> * MAC addressing
> * Error detection
> * Media access control
> * Switching
>
> This layer ensures that devices connected to the same local network can communicate properly and efficiently.

These notes cover:

* What is Data Link Layer
* Local communication
* Framing
* MAC addressing in detail
* NIC (Network Interface Card)
* LLC & MAC sublayer
* CRC and error detection
* CSMA/CD and CSMA/CA
* Switching and CAM table
* ARP basics
* Broadcast and collision domains
* Encapsulation at Layer 2
* Layer-2 attacks in cybersecurity

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why Data Link Layer Exists](#2-why-data-link-layer-exists)
* [3. Main Responsibilities](#3-main-responsibilities)
* [4. Local Communication Concept](#4-local-communication-concept)
* [5. Framing](#5-framing)
* [6. MAC Addressing](#6-mac-addressing)
* [7. NIC (Network Interface Card)](#7-nic-network-interface-card)
* [8. LLC & MAC Sublayer](#8-llc--mac-sublayer)
* [9. CRC and Error Detection](#9-crc-and-error-detection)
* [10. Media Access Control](#10-media-access-control)
* [11. Switching and CAM Table](#11-switching-and-cam-table)
* [12. Broadcast & Collision Domain](#12-broadcast--collision-domain)
* [13. ARP Basics](#13-arp-basics)
* [14. Encapsulation at Layer 2](#14-encapsulation-at-layer-2)
* [15. Layer-2 Cyber Attacks](#15-layer-2-cyber-attacks)
* [16. Quick Revision Sheet](#16-quick-revision-sheet)

---

# 1. Introduction

The Data Link Layer is:

```text
Layer 2 of OSI Model
```

It sits above:

```text
Physical Layer
```

Main purpose:

```text
Communication inside local network
```

This layer mainly works with:

```text
Frames
MAC Addresses
```

---

# 2. Why Data Link Layer Exists

Suppose two devices are connected to same switch or WiFi.

Questions arise:

* How does one device identify another?
* How are bits organized?
* How are corrupted bits detected?
* Who gets permission to transmit?
* How does switch know where to send frame?

Data Link Layer solves these problems.

---

# Important Understanding

| Layer           | Scope                |
| --------------- | -------------------- |
| Network Layer   | Global communication |
| Data Link Layer | Local communication  |

---

# 3. Main Responsibilities

| Responsibility       | Purpose                    |
| -------------------- | -------------------------- |
| Framing              | Organize bits              |
| MAC Addressing       | Local identification       |
| Error Detection      | Detect corruption          |
| Media Access Control | Control transmission       |
| Switching            | Local forwarding           |
| Local Delivery       | Same-network communication |

---

# 4. Local Communication Concept

Data Link Layer handles communication:

```text
Inside same local network
```

Example:

```text
Laptop ↔ Switch ↔ Printer
```

or:

```text
Phone ↔ WiFi Router
```

If communication goes outside local network:

```text
Router + Network Layer take control
```

---

# Important Understanding

Layer 2 communication uses:

```text
MAC addresses
```

Layer 3 communication uses:

```text
IP addresses
```

---

# 5. Framing

Data Link Layer groups bits into structures called:

```text
Frames
```

Without framing:

```text
Bits become difficult to interpret
```

---

# Why Framing Is Needed

Framing helps:

* identify start/end of data
* organize transmission
* detect corruption
* carry addressing information

---

# Basic Frame Structure

```text
Header
Payload(Data)
Trailer
```

---

# Header Contains

* Source MAC
* Destination MAC
* Control information

---

# Trailer Contains

Usually:

```text
CRC
```

for error detection.

---

# Important Understanding

| Layer     | PDU     |
| --------- | ------- |
| Transport | Segment |
| Network   | Packet  |
| Data Link | Frame   |
| Physical  | Bits    |

---

# 6. MAC Addressing

MAC means:

```text
Media Access Control Address
```

It is a unique hardware identifier used inside local network.

---

# MAC Address Format

Usually:

```text
48 bits
```

Written in hexadecimal.

Example:

```text
00:1A:2B:3C:4D:5E
```

---

# MAC Address Structure

```text
00:1A:2B  → Vendor/OUI
3C:4D:5E  → Device specific
```

---

# OUI

OUI means:

```text
Organizationally Unique Identifier
```

Assigned to hardware vendors.

Example:

| Vendor  | Example       |
| ------- | ------------- |
| Intel   | Intel NIC     |
| Realtek | Realtek NIC   |
| Cisco   | Cisco devices |

---

# Important MAC Types

| Type      | Purpose             |
| --------- | ------------------- |
| Unicast   | One device          |
| Broadcast | Entire network      |
| Multicast | Group communication |

---

# Broadcast MAC Address

```text
FF:FF:FF:FF:FF:FF
```

Meaning:

```text
Send to all devices
```

---

# Important Understanding

MAC addresses mainly work:

```text
Inside local network
```

Routers usually forward packets using:

```text
IP addresses
```

---

# 7. NIC (Network Interface Card)

NIC means:

```text
Network Interface Card
```

It allows device to connect to network.

---

# Responsibilities of NIC

* Stores MAC address
* Sends/receives frames
* Converts data into signals
* Handles local communication

---

# Types of NIC

| Type         | Example      |
| ------------ | ------------ |
| Wired NIC    | Ethernet     |
| Wireless NIC | WiFi Adapter |

---

# View MAC Address in Linux

```bash
ip a
```

or:

```bash
ifconfig
```

---

# 8. LLC & MAC Sublayer

Data Link Layer contains two sublayers:

```text
LLC
MAC
```

---

# LLC Sublayer

LLC means:

```text
Logical Link Control
```

Acts between:

```text
Network Layer
and
MAC Sublayer
```

---

# Responsibilities of LLC

* protocol identification
* flow control
* error management

---

# DSAP

Destination Service Access Point

Defines:

```text
Destination protocol
```

---

# SSAP

Source Service Access Point

Defines:

```text
Source protocol
```

---

# Control Field

Contains:

* acknowledgements
* frame type
* control information

---

# MAC Sublayer

MAC means:

```text
Media Access Control
```

Responsible for:

* MAC addressing
* transmission control
* collision handling
* frame delivery

---

# Important Understanding

| Sublayer | Main Work                    |
| -------- | ---------------------------- |
| LLC      | Logical communication        |
| MAC      | Physical/local communication |

---

# 9. CRC and Error Detection

CRC means:

```text
Cyclic Redundancy Check
```

Used to detect corrupted data.

---

# Why CRC Is Needed

During transmission:

* electrical noise
* interference
* signal distortion

may corrupt bits.

Example:

Sender:

```text
10101010
```

Receiver gets:

```text
10111010
```

Bit changed accidentally.

---

# How CRC Works

Sender side:

1. Frame data processed mathematically
2. CRC value generated
3. CRC added to trailer

Receiver side:

1. Receiver recalculates CRC
2. Compares with received CRC
3. If mismatch:

```text
Corruption detected
```

---

# Important Understanding

CRC mainly:

```text
Detects errors
```

Usually it does NOT:

```text
Recover errors
```

Recovery generally handled at higher layers.

---

# Simplified CRC Flow

```text
Data
↓
CRC Generated
↓
Frame Sent
↓
Receiver Calculates CRC
↓
Compare CRC
↓
Valid or Corrupted
```

---

# Why CRC Is Powerful

CRC can detect:

* single-bit errors
* burst errors
* accidental corruption

Very common in:

* Ethernet
* WiFi
* storage systems

---

# 10. Media Access Control

Suppose many devices share same medium:

```text
WiFi
Ethernet
```

Question:

```text
Who transmits first?
```

MAC sublayer controls this.

---

# Why Needed?

Without coordination:

* collisions occur
* signals overlap
* communication fails

---

# A. CSMA/CD

Meaning:

```text
Carrier Sense Multiple Access
with Collision Detection
```

Used mainly in older Ethernet.

---

# Working

1. Device checks medium
2. If free → transmit
3. If collision occurs:

   * stop transmission
   * wait random time
   * retransmit

---

# B. CSMA/CA

Meaning:

```text
Collision Avoidance
```

Used mainly in WiFi.

Instead of detecting collisions:

```text
Attempts to avoid them
```

---

# Why WiFi Uses CA Instead of CD

Wireless devices cannot reliably detect collisions while transmitting.

So WiFi mainly tries to:

```text
Prevent collisions
```

---

# 11. Switching and CAM Table

Switch operates mainly at:

```text
Layer 2
```

---

# Main Job of Switch

Forward frames using:

```text
MAC addresses
```

---

# CAM Table

Switch stores:

```text
MAC Address ↔ Port Mapping
```

Example:

| MAC Address | Port   |
| ----------- | ------ |
| AA:BB:CC    | Port 1 |
| DD:EE:FF    | Port 2 |

---

# Switching Process

1. Frame enters switch
2. Switch learns source MAC
3. Switch checks destination MAC
4. Frame forwarded to correct port

---

# Unknown Destination

If destination MAC unknown:

```text
Switch floods frame
```

to all ports except sender.

---

# Important Understanding

Switch mainly cares about:

```text
MAC addresses
```

not:

* websites
* applications
* user data meaning

---

# 12. Broadcast & Collision Domain

---

# Broadcast Domain

Broadcast traffic reaches:

```text
All devices
```

inside broadcast domain.

Routers separate broadcast domains.

---

# Collision Domain

Collision domain means:

```text
Area where packet collisions may occur
```

---

# Important Understanding

| Device | Collision Domain                   |
| ------ | ---------------------------------- |
| Hub    | Single collision domain            |
| Switch | Separate collision domain per port |

---

# Why Switch Is Better Than Hub

Switch reduces:

```text
Collisions
```

improving network efficiency.

---

# 13. ARP Basics

ARP means:

```text
Address Resolution Protocol
```

Purpose:

```text
IP Address → MAC Address
```

---

# Example

Suppose:

```text
192.168.1.10
```

known but MAC unknown.

Device broadcasts:

```text
Who has 192.168.1.10?
```

Target replies with MAC address.

---

# ARP Cache

Devices temporarily store:

```text
IP ↔ MAC mapping
```

View in Linux:

```bash
arp -a
```

or:

```bash
ip neigh
```

---

# Important Understanding

ARP works mainly at:

```text
Layer 2 / Layer 3 boundary
```

---

# 14. Encapsulation at Layer 2

At this layer:

```text
Packet becomes Frame
```

---

# Encapsulation Process

Network Layer sends:

```text
Packet
```

Data Link Layer adds:

```text
MAC Header
Trailer
```

Result:

```text
Frame
```

---

# Simplified Structure

```text
MAC Header
+
IP Packet
+
CRC Trailer
```

---

# Receiver Side

Receiver removes:

* MAC Header
* CRC Trailer

Then passes packet upward.

---

# 15. Layer-2 Cyber Attacks

---

# ARP Spoofing

Attacker sends fake ARP replies.

Result:

```text
Man-in-the-Middle Attack
```

---

# MAC Flooding

Attacker floods switch with fake MAC addresses.

Result:

```text
Switch behaves like hub
```

Traffic sniffing becomes possible.

---

# VLAN Hopping

Attacker escapes VLAN restrictions.

Result:

```text
Unauthorized network access
```

---

# Rogue Access Point

Attacker creates fake WiFi AP.

Result:

```text
Traffic interception
Credential theft
```

---

# DHCP Starvation

Attacker consumes all DHCP addresses.

Result:

```text
Denial of service
```

---

# STP Manipulation

Attacker manipulates spanning tree behavior.

Result:

```text
Traffic redirection
MITM
```

---

# MAC Spoofing

Attacker changes device MAC address.

Used for:

* bypass filtering
* impersonation
* anonymity

---

# 16. Quick Revision Sheet

```text
Layer 2 = Local communication
```

---

# Main Responsibilities

```text
Framing
MAC Addressing
Error Detection
Media Access Control
Switching
```

---

# Important Concepts

```text
Frame = Layer-2 PDU

MAC = Local identity

IP = Global identity
```

---

# Important Devices

```text
Switch → Layer 2

Bridge → Layer 2

NIC → Layer 2
```

---

# Sublayers

```text
LLC → Logical communication

MAC → Media access & addressing
```

---

# Biggest Concept

```text
Data Link Layer handles
device-to-device communication
inside local network
```

---

*End of Data Link Layer Notes*
