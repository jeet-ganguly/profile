# Computer Networks — Overview of Transport Layer

> The Transport Layer is Layer 4 of the OSI Model and provides end-to-end communication between processes running on different hosts.
>
> It is responsible for reliable delivery, segmentation, flow control, error control, multiplexing, and process-to-process communication.
>
> The two major protocols operating at this layer are TCP and UDP.

These notes cover:

* Introduction to Transport Layer
* Functions of Transport Layer
* Process-to-Process Communication
* Segmentation and Reassembly
* Port Numbers
* Multiplexing and Demultiplexing
* Flow Control
* Error Control
* Connection-Oriented and Connectionless Services
* TCP and UDP
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Position in OSI and TCP/IP Model](#2-position-in-osi-and-tcpip-model)
* [3. Why Transport Layer is Needed](#3-why-transport-layer-is-needed)
* [4. Functions of Transport Layer](#4-functions-of-transport-layer)
* [5. Process-to-Process Communication](#5-process-to-process-communication)
* [6. Segmentation and Reassembly](#6-segmentation-and-reassembly)
* [7. Port Numbers](#7-port-numbers)
* [8. Multiplexing and Demultiplexing](#8-multiplexing-and-demultiplexing)
* [9. Flow Control](#9-flow-control)
* [10. Error Control](#10-error-control)
* [11. Connection-Oriented and Connectionless Services](#11-connection-oriented-and-connectionless-services)
* [12. TCP and UDP](#12-tcp-and-udp)
* [13. Cybersecurity Perspective](#13-cybersecurity-perspective)
* [14. Quick Revision Sheet](#14-quick-revision-sheet)

---

# 1. Introduction

The Transport Layer is:

```text
Layer 4
```

of the OSI Model.

It provides:

```text
Process-to-Process
Communication
```

between applications running on different devices.

---

## Example

```text
Web Browser
      ↓
Transport Layer
      ↓
Network Layer
      ↓
Internet
      ↓
Network Layer
      ↓
Transport Layer
      ↓
Web Server
```

---

# 2. Position in OSI and TCP/IP Model

## OSI Model

```text
Application Layer
Presentation Layer
Session Layer
-----------------
Transport Layer
-----------------
Network Layer
Data Link Layer
Physical Layer
```

---

## TCP/IP Model

```text
Application Layer
-----------------
Transport Layer
-----------------
Internet Layer
Network Access Layer
```

---

# 3. Why Transport Layer is Needed

The Network Layer provides:

```text
Host-to-Host Communication
```

But applications require:

```text
Process-to-Process Communication
```

For example:

```text
One Computer
```

may simultaneously run:

* Chrome
* SSH
* Email
* FTP

The Transport Layer ensures that data reaches the correct application.

---

# 4. Functions of Transport Layer

Major functions include:

```text
Segmentation

Reassembly

Port Addressing

Flow Control

Error Control

Multiplexing

Demultiplexing

Reliable Communication
```

---

# 5. Process-to-Process Communication

The Network Layer identifies:

```text
Destination Device
```

using:

```text
IP Address
```

The Transport Layer identifies:

```text
Destination Process
```

using:

```text
Port Numbers
```

---

## Example

```text
192.168.1.10
```

identifies the computer.

```text
80
```

identifies the web service.

Therefore:

```text
192.168.1.10:80
```

uniquely identifies a process.

---

# 6. Segmentation and Reassembly

Large messages are divided into smaller pieces called:

```text
Segments
```

---

## Segmentation

Sender:

```text
Large File
     ↓
Segment 1

Segment 2

Segment 3
```

---

## Reassembly

Receiver:

```text
Segment 1

Segment 2

Segment 3
     ↓
Original File
```

---

## Benefits

* Easier transmission
* Better reliability
* Efficient error recovery

---

# 7. Port Numbers

Port numbers uniquely identify applications.

---

## Port Number Size

```text
16 Bits
```

Range:

```text
0 - 65535
```

---

## Types of Ports

### Well-Known Ports

Range:

```text
0 - 1023
```

Examples:

| Port | Service     |
| ---- | ----------- |
| 20   | FTP Data    |
| 21   | FTP Control |
| 22   | SSH         |
| 23   | Telnet      |
| 25   | SMTP        |
| 53   | DNS         |
| 80   | HTTP        |
| 110  | POP3        |
| 143  | IMAP        |
| 443  | HTTPS       |

---

### Registered Ports

Range:

```text
1024 - 49151
```

Used by:

* Applications
* Vendors

---

### Dynamic Ports

Range:

```text
49152 - 65535
```

Used temporarily by clients.

---

# 8. Multiplexing and Demultiplexing

---

## Multiplexing

Multiple applications send data simultaneously.

```text
Browser
SSH
Email
FTP
   ↓
Transport Layer
```

The Transport Layer combines them before sending.

---

## Demultiplexing

Receiver separates incoming data and delivers it to the correct application.

```text
Incoming Segments
       ↓
Transport Layer
       ↓
Browser

SSH

Email

FTP
```

---

# 9. Flow Control

Flow control prevents:

```text
Fast Sender
```

from overwhelming:

```text
Slow Receiver
```

---

## Example

Without flow control:

```text
Sender
↓↓↓↓↓↓↓↓↓↓↓↓↓↓↓

Receiver
```

Receiver may lose packets.

---

With flow control:

```text
Sender
↓
↓
↓
Receiver
```

data transmission becomes manageable.

---

## Purpose

* Prevent buffer overflow
* Improve efficiency
* Maintain stability

---

# 10. Error Control

Error control ensures reliable delivery.

---

## Responsibilities

* Detect lost segments
* Recover lost segments
* Ensure correct sequence

---

## Mechanisms Used

* Acknowledgments
* Sequence Numbers
* Retransmissions

---

## Example

```text
Segment 1 ✔

Segment 2 Lost

Segment 3 ✔
```

Receiver requests retransmission of:

```text
Segment 2
```

---

# 11. Connection-Oriented and Connectionless Services

---

## Connection-Oriented Service

Communication begins by establishing a connection.

Characteristics:

* Reliable
* Ordered Delivery
* Error Recovery

Example:

```text
TCP
```

---

## Connectionless Service

No connection setup is required.

Characteristics:

* Faster
* Less Overhead
* No Delivery Guarantee

Example:

```text
UDP
```

---

# 12. TCP and UDP

---

## TCP

TCP stands for:

```text
Transmission Control Protocol
```

Characteristics:

* Reliable
* Connection-Oriented
* Error Recovery
* Ordered Delivery

Applications:

* HTTP
* HTTPS
* SSH
* FTP
* Email

---

## UDP

UDP stands for:

```text
User Datagram Protocol
```

Characteristics:

* Connectionless
* Faster
* No Acknowledgment
* Lower Overhead

Applications:

* DNS
* VoIP
* Online Gaming
* Video Streaming

---

# Comparison of TCP and UDP

| Feature        | TCP                 | UDP            |
| -------------- | ------------------- | -------------- |
| Connection     | Connection-Oriented | Connectionless |
| Reliability    | Reliable            | Unreliable     |
| Speed          | Slower              | Faster         |
| Acknowledgment | Yes                 | No             |
| Ordering       | Guaranteed          | Not Guaranteed |
| Header Size    | 20–60 Bytes         | 8 Bytes        |

---

# 13. Cybersecurity Perspective

---

## Port Scanning

Attackers often perform:

```text
Port Scanning
```

to discover:

* Open Services
* Running Applications

Tools:

* Nmap
* Masscan

---

## SYN Flood Attack

Targets:

```text
TCP Connection Establishment
```

Consumes server resources.

---

## UDP Flood Attack

Overwhelms systems with:

```text
UDP Packets
```

causing denial of service.

---

## Session Hijacking

Attackers attempt to steal:

```text
TCP Sessions
```

to impersonate legitimate users.

---

## Commonly Targeted Ports

| Port | Service |
| ---- | ------- |
| 21   | FTP     |
| 22   | SSH     |
| 23   | Telnet  |
| 53   | DNS     |
| 80   | HTTP    |
| 443  | HTTPS   |
| 445  | SMB     |
| 3389 | RDP     |

---

# 14. Quick Revision Sheet

Layer:

```text
Layer 4
```

---

Provides:

```text
Process-to-Process Communication
```

---

Major Functions:

```text
Segmentation

Reassembly

Flow Control

Error Control

Port Addressing

Multiplexing

Demultiplexing
```

---

Port Number Size:

```text
16 Bits
```

Range:

```text
0 - 65535
```

---

Protocols:

```text
TCP

UDP
```

---

TCP:

```text
Reliable

Connection-Oriented
```

---

UDP:

```text
Fast

Connectionless
```

---

Biggest Concept:

```text
Network Layer delivers data
to the correct host.

Transport Layer delivers data
to the correct process.
```

---

*End of Overview of Transport Layer Notes*
