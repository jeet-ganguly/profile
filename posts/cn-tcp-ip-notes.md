# Computer Networks — TCP/IP Stack

> The TCP/IP stack is a layered networking architecture that explains how data travels from one device to another across local networks and the Internet.
>
> Instead of one giant system handling communication, networking divides responsibilities into layers.

These notes cover:

* Why TCP/IP stack exists
* Layered networking concept
* Encapsulation & Decapsulation
* Peer-to-peer communication
* Application Layer
* Transport Layer
* Network Layer
* Data Link Layer
* Physical Layer
* Device mapping
* TCP/IP vs OSI
* Complete packet journey

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why TCP/IP Stack Exists](#2-why-tcpip-stack-exists)
* [3. TCP/IP Layer Overview](#3-tcpip-layer-overview)
* [4. Core Layer Philosophy](#4-core-layer-philosophy)
* [5. Data Flow Overview](#5-data-flow-overview)
* [6. Encapsulation & Decapsulation](#6-encapsulation--decapsulation)
* [7. Peer-to-Peer Communication](#7-peer-to-peer-communication)
* [8. Application Layer](#8-application-layer)
* [9. Transport Layer](#9-transport-layer)
* [10. Network Layer](#10-network-layer)
* [11. Data Link Layer](#11-data-link-layer)
* [12. Physical Layer](#12-physical-layer)
* [13. Device Mapping](#13-device-mapping)
* [14. TCP/IP vs OSI Model](#14-tcpip-vs-osi-model)
* [15. Complete Communication Flow](#15-complete-communication-flow)
* [16. Quick Revision Sheet](#16-quick-revision-sheet)

---

# 1. Introduction

TCP/IP Stack is the networking architecture used by the Internet.

Important:

```text id="m2q8tb"
TCP/IP = Practical implementation of networking
```

Main idea:

```text id="rmbjdp"
Each layer performs one specific task
and passes data to another layer.
```

This makes networking:

* modular
* scalable
* easier to debug
* easier to standardize

---

# 2. Why TCP/IP Stack Exists

Suppose you send:

```text id="ly8hcm"
"Hello"
```

To deliver this message:

* application creates data
* destination must be identified
* routing must happen
* local network must transfer data
* electrical/radio signals must carry bits

If one system handled everything:

* networking would become extremely complex
* debugging would become difficult
* scalability would be poor

So networking uses:

```text id="mrm96t"
Layered abstraction
```

Each layer focuses only on its own responsibility.

---

# 3. TCP/IP Layer Overview

```text id="jq01nt"
5. Application Layer
4. Transport Layer
3. Network Layer
2. Data Link Layer
1. Physical Layer
```

---

# Main Responsibilities

| Layer       | Responsibility              |
| ----------- | --------------------------- |
| Application | User communication          |
| Transport   | Process-to-process delivery |
| Network     | Routing between devices     |
| Data Link   | Local network communication |
| Physical    | Bit transmission            |

---

# Important Concept

Upper layers focus more on:

```text id="h8x1o7"
Logic and communication
```

Lower layers focus more on:

```text id="7u8zzp"
Transmission and hardware
```

---

# 4. Core Layer Philosophy

Every layer:

* receives data from upper layer
* adds its own information
* passes data downward

Receiver side:

* removes layer information
* extracts original data
* passes data upward

This creates:

```text id="t10kxy"
Modular communication
```

---

# 5. Data Flow Overview

## Sender Side

```text id="td0s5n"
Application
↓
Transport
↓
Network
↓
Data Link
↓
Physical
```

---

## Receiver Side

```text id="tr6ll0"
Physical
↑
Data Link
↑
Network
↑
Transport
↑
Application
```

---

## Important Terms

Sender process:

```text id="t7p4ri"
Encapsulation
```

Receiver process:

```text id="m4v9r4"
Decapsulation
```

---

# 6. Encapsulation & Decapsulation

## Encapsulation

Each layer wraps data inside another structure.

Example:

Application Layer:

```text id="o6cm2w"
Data
```

Transport Layer:

```text id="7z7d2s"
Transport Header + Data
```

Network Layer:

```text id="if40xj"
IP Header + Transport Header + Data
```

Data Link Layer:

```text id="yn4f6f"
MAC Header + IP Header + Transport Header + Data
```

Physical Layer:

```text id="0ndt7v"
010101001010...
```

---

## Decapsulation

Receiver removes headers layer-by-layer.

```text id="qm02g0"
Bits
↓
Frames
↓
Packets
↓
Segments
↓
Application Data
```

---

# 7. Peer-to-Peer Communication

Important Concept:

```text id="ejzjlwm"
Each layer logically communicates
with the same layer on another device
```

Examples:

* Application ↔ Application
* Transport ↔ Transport
* Network ↔ Network

Even though actual data travels through all layers.

---

# 8. Application Layer

Closest layer to user.

Provides communication services to applications.

---

# Main Goal

```text id="7jlwmv"
Enable software applications
to communicate
```

---

# What Happens Here?

Applications generate logical data.

Examples:

* Browser requests webpage
* Chat application sends message
* Email client sends email
* SSH terminal sends commands

---

# Responsibilities

## A. User Interaction

Acts as interface between user and network.

---

## B. Communication Rules

Defines:

* request format
* response format
* error handling

---

## C. Data Representation

Ensures applications understand data properly.

---

## D. Session Handling

Controls communication sessions.

---

# Important Understanding

Application Layer handles:

```text id="jlwm88"
Communication logic
```

It does NOT handle:

* routing
* signals
* MAC addresses
* electrical transmission

---

# 9. Transport Layer

Responsible for:

```text id="jlwmx2"
End-to-end communication
between applications
```

---

# Main Purpose

Suppose one computer runs:

* Browser
* SSH terminal
* Chat application
* Music application

Transport Layer identifies:

```text id="jlwmp7"
Which data belongs
to which application
```

---

# Responsibilities

| Responsibility | Purpose                        |
| -------------- | ------------------------------ |
| Segmentation   | Break data into smaller pieces |
| Reliability    | Ensure delivery                |
| Flow Control   | Prevent overload               |
| Error Recovery | Handle packet loss             |
| Multiplexing   | Handle multiple applications   |

---

# Process-to-Process Delivery

Network Layer delivers data:

```text id="jlwmv4"
To a device
```

Transport Layer delivers data:

```text id="jlwm9w"
To a specific application
```

---

# Ports

Applications are identified using:

```text id="jlwmg5"
Port numbers
```

Examples:

| Service | Port |
| ------- | ---- |
| HTTP    | 80   |
| HTTPS   | 443  |
| SSH     | 22   |

---

# Reliability Concept

Reliable communication may include:

* acknowledgements
* retransmission
* packet ordering
* loss detection

---

# Important Understanding

Transport Layer mainly handles:

```text id="jlwmd1"
Communication quality
```

---

# 10. Network Layer

Responsible for:

```text id="jlwm0z"
Routing packets across networks
```

---

# Main Goal

Find path between source and destination devices.

---

# Responsibilities

| Responsibility     | Purpose              |
| ------------------ | -------------------- |
| Logical Addressing | Identify devices     |
| Routing            | Select path          |
| Packet Forwarding  | Move packets         |
| Fragmentation      | Handle large packets |

---

# Logical Addressing

Devices require unique identifiers.

Examples:

```text id="jlwm2m"
IPv4
IPv6
```

---

# Routing

Routing means:

```text id="jlwmr9"
Choosing path across multiple networks
```

Routers mainly operate here.

---

# Important Understanding

Network Layer only cares about:

```text id="jlwmf7"
Where packet should go
```

It does NOT care about:

* user data meaning
* applications
* file content

---

# Packet Switching

Internet uses:

```text id="jlwmt2"
Packet switching
```

Data is divided into packets.

Different packets may use different paths.

---

# Best-Effort Delivery

Network Layer usually provides:

```text id="jlwmq3"
Best-effort delivery
```

Meaning:

* no guarantee
* no reliability
* no ordering guarantee

Reliability mainly belongs to Transport Layer.

---

# 11. Data Link Layer

Responsible for:

```text id="jlwm44"
Communication inside local network
```

---

# Scope Difference

| Layer     | Scope           |
| --------- | --------------- |
| Network   | Internet/global |
| Data Link | Local network   |

---

# Responsibilities

| Responsibility       | Purpose                   |
| -------------------- | ------------------------- |
| Framing              | Organize data             |
| MAC Addressing       | Local identification      |
| Error Detection      | Detect corruption         |
| Media Access Control | Share transmission medium |

---

# Framing

Groups bits into:

```text id="jlwm6f"
Frames
```

---

# MAC Addressing

Devices communicate locally using:

```text id="jlwm88"
MAC addresses
```

Important:

```text id="jlwm92"
MAC → Local identity

IP → Global identity
```

---

# Media Access Control

Controls:

```text id="jlwma8"
Who can transmit data
on shared medium
```

Examples:

* WiFi
* Ethernet

---

# Error Detection

Detects corruption during local communication.

Usually:

```text id="jlwmv0"
Detects errors
but does not recover them
```

---

# Devices Here

| Device |
| ------ |
| Switch |
| Bridge |

---

# 12. Physical Layer

Lowest layer.

Responsible for:

```text id="jlwms9"
Actual transmission of bits
```

---

# What Happens Here?

Logical data becomes:

```text id="jlwmk1"
Electrical signals
Optical signals
Radio signals
```

---

# Responsibilities

| Responsibility           | Purpose                     |
| ------------------------ | --------------------------- |
| Signal generation        | Create physical signals     |
| Bit transmission         | Send bits                   |
| Physical connection      | Cable/wireless transmission |
| Timing & synchronization | Maintain transmission rules |

---

# Physical Mediums

Examples:

| Medium         |
| -------------- |
| Ethernet cable |
| Fiber optic    |
| WiFi radio     |
| Bluetooth      |

---

# Devices Here

| Device   |
| -------- |
| Hub      |
| Repeater |

---

# Important Understanding

Physical Layer only understands:

```text id="jlwm20"
0s and 1s
```

It does NOT understand:

* applications
* packets
* frames
* addresses

---

# 13. Device Mapping

| Device   | Layer       |
| -------- | ----------- |
| Hub      | Physical    |
| Repeater | Physical    |
| Switch   | Data Link   |
| Router   | Network     |
| Firewall | Layer 3–7   |
| Browser  | Application |

---

# 14. TCP/IP vs OSI Model

| OSI          | TCP/IP      |
| ------------ | ----------- |
| Application  | Application |
| Presentation | Application |
| Session      | Application |
| Transport    | Transport   |
| Network      | Network     |
| Data Link    | Data Link   |
| Physical     | Physical    |

---

# Important Concept

TCP/IP combines:

```text id="jlwmtq"
Application
Presentation
Session
```

into one Application Layer.

---

# 15. Complete Communication Flow

Suppose browser sends request.

---

## Step 1 — Application Layer

Creates logical data.

---

## Step 2 — Transport Layer

Breaks data into segments and manages delivery.

---

## Step 3 — Network Layer

Adds addressing and routing information.

---

## Step 4 — Data Link Layer

Prepares frame for local communication.

---

## Step 5 — Physical Layer

Converts bits into physical signals.

---

# Receiver Side

Receiver performs reverse process:

```text id="jlwmv9"
Signals
→ Frames
→ Packets
→ Segments
→ Application Data
```

---

# 16. Quick Revision Sheet

```text id="jlwmq7"
Application → User communication

Transport → Application delivery

Network → Routing

Data Link → Local delivery

Physical → Signal transmission
```

---

# Address Types

```text id="jlwmn1"
Port Number → Application

IP Address → Device identity

MAC Address → Local hardware identity
```

---

# Important Devices

```text id="jlwmd8"
Hub → Physical

Switch → Data Link

Router → Network
```

---

# Biggest Concept

```text id="jlwmx0"
TCP/IP Stack transforms
human-readable data
into physical signals
and back again.
```

---

*End of TCP/IP Stack Notes*
