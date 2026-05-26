# Computer Networking Notes — OSI Model

## Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why OSI Exists](#2-why-osi-exists)
* [3. OSI Layer Overview](#3-osi-layer-overview)
* [4. Data Flow Overview](#4-data-flow-overview)
* [5. Encapsulation & Decapsulation](#5-encapsulation--decapsulation)
* [6. Protocol Data Unit (PDU)](#6-protocol-data-unit-pdu)
* [7. OSI Layers Explained](#7-osi-layers-explained)
* [8. Device Mapping](#8-device-mapping)
* [9. LLC & MAC Sublayer](#9-llc--mac-sublayer)
* [10. Layer-wise Cyber Attacks](#10-layer-wise-cyber-attacks)
* [11. Quick Revision Sheet](#11-quick-revision-sheet)

---

# 1. Introduction

OSI stands for:

```text
Open Systems Interconnection
```

It is a conceptual networking model that standardizes communication between systems.

Before OSI:

* Vendors used proprietary protocols
* Different systems could not communicate properly
* Compatibility issues existed

OSI introduced a common framework for communication.

Important:

```text
OSI = Theoretical model

TCP/IP = Practical implementation
```

TCP/IP uses OSI concepts but merges several layers.

---

# 2. Why OSI Exists

Earlier:

```text
Computer A (Vendor A)
       ↓
Incompatible protocols
       ↓
Computer B (Vendor B)
```

Problems:

* Different network standards
* Different packet structures
* Difficult troubleshooting

OSI solved this by introducing:

```text
Layered communication
```

Benefits:

* Easier troubleshooting
* Standardized communication
* Modular architecture
* Better protocol understanding
* Helps security professionals locate attacks

---

# 3. OSI Layer Overview

```text
7 Application
6 Presentation
5 Session
4 Transport
3 Network
2 Data Link
1 Physical
```

Memory trick:

```text
All People Seem To Need Data Processing
```

Reverse:

```text
Please Do Not Throw Sausage Pizza Away
```

---

# 4. Data Flow Overview

Sender:

```text
Application
↓
Presentation
↓
Session
↓
Transport
↓
Network
↓
Data Link
↓
Physical
```

Transmission:

```text
Bits move across medium
```

Receiver:

```text
Physical
↑
Data Link
↑
Network
↑
Transport
↑
Session
↑
Presentation
↑
Application
```

Sender:

```text
Encapsulation
```

Receiver:

```text
Decapsulation
```

---

# 5. Encapsulation & Decapsulation

## Encapsulation

Every layer adds its own information:

```text
Header
```

Data becomes larger while moving downward.

Example:

Application:

```text
Data
```

Transport:

```text
TCP Header + Data
```

Network:

```text
IP Header + TCP Header + Data
```

Data Link:

```text
MAC Header + IP Header + TCP Header + Data
```

Physical:

```text
01001010100101...
```

---

## Decapsulation

Receiver removes headers layer-by-layer:

```text
Remove Header
↓
Extract Data
↓
Pass upward
```

---

# 6. Protocol Data Unit (PDU)

Different layers use different names.

| Layer        | PDU     |
| ------------ | ------- |
| Application  | Data    |
| Presentation | Data    |
| Session      | Data    |
| Transport    | Segment |
| Network      | Packet  |
| Data Link    | Frame   |
| Physical     | Bits    |

---

# 7. OSI Layers Explained

# Layer 7 — Application Layer

Closest layer to users.

Provides network services directly to applications.

Examples:

* Browser
* Email client
* SSH client
* DNS request

Protocols:

```text
HTTP
HTTPS
FTP
SMTP
SSH
DNS
SNMP
Telnet
```

Responsibilities:

* File transfer
* Web communication
* Email handling
* User authentication

Examples:

```text
Chrome
Firefox
curl
SSH
```

---

# Layer 6 — Presentation Layer

Responsible for:

```text
Data representation
```

Makes sure sender and receiver understand data identically.

Examples:

```text
TLS
SSL
ASCII
Unicode
PNG
JPEG
MP3
```

Responsibilities:

* Encryption/Decryption
* Compression
* Encoding/Decoding

---

# Layer 5 — Session Layer

Creates and manages communication sessions.

Examples:

```text
Cookies
Session IDs
JWT Tokens
RPC Sessions
```

Responsibilities:

* Session establishment
* Session maintenance
* Session termination

---

# Layer 4 — Transport Layer

Provides:

```text
Reliable end-to-end communication
```

Protocols:

```text
TCP
UDP
```

Responsibilities:

* Segmentation
* Reliability
* Flow control
* Error handling
* Retransmission

Common ports:

```text
HTTP → 80
HTTPS → 443
SSH → 22
DNS → 53
```

---

# Layer 3 — Network Layer

Responsible for:

```text
IP addressing
Routing
Path selection
```

Protocols:

```text
IPv4
IPv6
ICMP
IPsec
```

Device:

```text
Router
```

Responsibilities:

* Logical addressing
* Route selection
* Packet forwarding

---

# Layer 2 — Data Link Layer

Transfers data inside local network.

Uses:

```text
MAC Address
```

Contains two sublayers:

```text
MAC
LLC
```

Protocols:

```text
Ethernet
ARP
PPP
```

Devices:

```text
Switch
Bridge
```

Responsibilities:

* Framing
* Error detection
* MAC addressing
* Local delivery

Error detection:

```text
CRC
```

Older Ethernet:

```text
CSMA/CD
```

---

# Layer 1 — Physical Layer

Handles actual transmission of signals.

Deals with:

* Voltage
* Radio frequency
* Fiber optics
* Cables

Examples:

```text
Ethernet cable
Fiber cable
WiFi radio
```

Devices:

```text
Hub
Repeater
```

Works only with:

```text
Bits
```

---

# 8. Device Mapping

| Device   | OSI Layer   |
| -------- | ----------- |
| Hub      | Physical    |
| Repeater | Physical    |
| Switch   | Data Link   |
| Router   | Network     |
| Firewall | Layer 3–7   |
| Proxy    | Application |
| Browser  | Application |

---

# 9. LLC & MAC Sublayer

Data Link contains:

```text
LLC
MAC
```

---

## LLC

Logical Link Control

Acts as interface between:

```text
Network Layer
and
MAC layer
```

Responsibilities:

* Identifies destination protocol
* Flow control
* Error management

Examples:

```text
IPv4
IPv6
ARP
IPX
```

LLC header fields:

### DSAP

Destination Service Access Point

Determines:

```text
Destination protocol
```

---

### SSAP

Source Service Access Point

Determines:

```text
Source protocol
```

---

### Control Field

Contains:

```text
Acknowledgements
Frame type
Control information
```

---

## MAC

Media Access Control

Responsibilities:

* Frame construction
* MAC addressing
* Access to transmission medium

---

# 10. Layer-wise Cyber Attacks

After understanding the OSI model, security professionals often map attacks to specific layers.

---

## Layer 7 — Application Attacks

### HTTP Request Smuggling

Exploits parser differences between servers.

Impact:

```text
Cache poisoning
Authentication bypass
```

---

### API Abuse

Examples:

```text
IDOR
Broken Authentication
Business Logic attacks
```

---

## Layer 6 — Presentation Attacks

### SSL Stripping

Downgrades:

```text
HTTPS → HTTP
```

Result:

Traffic interception

---

### BREACH / CRIME

Compression-based attacks against encrypted traffic.

---

## Layer 5 — Session Attacks

### Session Hijacking

Steals:

```text
Session Cookies
JWT Tokens
```

Result:

```text
Account takeover
```

---

### Token Replay Attack

Captured session reused by attacker.

---

## Layer 4 — Transport Attacks

### TCP SYN Flood

Attacker sends:

```text
Massive SYN requests
```

without completing handshake.

Result:

```text
Resource exhaustion
```

---

### UDP Amplification Attack

Examples:

```text
DNS amplification
NTP amplification
Memcached amplification
```

---

## Layer 3 — Network Attacks

### BGP Route Hijacking

Fake route advertisements.

Impact:

```text
Traffic redirection
MITM
```

---

### ICMP Tunneling

Hides data inside ICMP packets.

Used by:

```text
Malware
APT groups
C2 channels
```

---

### Rogue IPv6 Router Advertisement

Fake IPv6 routes cause:

```text
MITM attacks
```

---

## Layer 2 — Data Link Attacks

### ARP Spoofing

Attacker sends fake ARP replies.

Result:

```text
MITM
```

Tools:

```text
bettercap
ettercap
arpspoof
```

---

### VLAN Hopping

Escape VLAN restrictions.

---

### MAC Flooding

Overflow switch CAM table.

Result:

```text
Switch behaves like hub
```

---

## Layer 1 — Physical Attacks

### Hardware Network Tap

Physical interception device inserted into network cable.

---

### TEMPEST Attacks

Captures electromagnetic leakage.

---

### Malicious USB Attacks

Examples:

```text
BadUSB
Rubber Ducky
```

---

# 11. Quick Revision Sheet

```text
Layer 7 → User applications

Layer 6 → Encryption

Layer 5 → Sessions

Layer 4 → TCP/UDP

Layer 3 → IP Routing

Layer 2 → MAC Address

Layer 1 → Bits
```

### Device Mapping:

```text
Hub → Layer1
Switch → Layer2
Router → Layer3
Firewall → Layer3–7
Browser → Layer7
```

### PDU:

```text
Data
Segment
Packet
Frame
Bits
```

---

End of Notes.
