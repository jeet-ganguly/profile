# Computer Networks — TCP Error Control Basics

> TCP (Transmission Control Protocol) is a reliable transport layer protocol.
>
> One of its most important responsibilities is error control.
>
> TCP ensures that data reaches the destination completely, correctly, and in the proper order even when packets are lost, duplicated, corrupted, or delivered out of sequence.

These notes cover:

* What is TCP Error Control
* Why Error Control is Needed
* Types of Transmission Errors
* Sequence Numbers
* Acknowledgments
* Retransmission
* Timeout Mechanism
* Duplicate Acknowledgments
* Fast Retransmission
* Checksum
* Sliding Window
* Cybersecurity Perspective

---

# Table of Contents

* [1. Introduction](#1-introduction)
* [2. Why Error Control is Needed](#2-why-error-control-is-needed)
* [3. Types of Errors in TCP Communication](#3-types-of-errors-in-tcp-communication)
* [4. Sequence Numbers](#4-sequence-numbers)
* [5. Acknowledgments (ACK)](#5-acknowledgments-ack)
* [6. Retransmission](#6-retransmission)
* [7. Timeout Mechanism](#7-timeout-mechanism)
* [8. Duplicate ACK](#8-duplicate-ack)
* [9. Fast Retransmission](#9-fast-retransmission)
* [10. TCP Checksum](#10-tcp-checksum)
* [11. Sliding Window and Reliability](#11-sliding-window-and-reliability)
* [12. Summary of TCP Error Control Process](#12-summary-of-tcp-error-control-process)
* [13. Cybersecurity Perspective](#13-cybersecurity-perspective)
* [14. Quick Revision Sheet](#14-quick-revision-sheet)

---

# 1. Introduction

TCP provides:

```text
Reliable Data Delivery
```

between two hosts.

To achieve reliability TCP performs:

```text
Error Detection

Lost Packet Recovery

Duplicate Packet Detection

Out-of-Order Packet Handling
```

This collection of mechanisms is called:

```text
TCP Error Control
```

---

# 2. Why Error Control is Needed

While travelling across a network:

```text
TCP Segment
      ↓
Internet
      ↓
Receiver
```

many problems may occur.

Examples:

* Packet Loss
* Packet Corruption
* Packet Duplication
* Out-of-Order Delivery
* Network Congestion

TCP must detect and recover from these issues.

---

# 3. Types of Errors in TCP Communication

---

## Packet Loss

A segment never reaches the receiver.

Example:

```text
Segment 1 ✔

Segment 2 ✖ Lost

Segment 3 ✔
```

---

## Packet Corruption

Data changes during transmission.

Example:

```text
Original

HELLO
```

becomes:

```text
HEXXO
```

---

## Duplicate Packet

The same packet arrives multiple times.

Example:

```text
Segment 1

Segment 1

Segment 2
```

---

## Out-of-Order Delivery

Packets arrive in the wrong order.

Example:

```text
Segment 1

Segment 3

Segment 2
```

TCP must rearrange them correctly.

---

# 4. Sequence Numbers

TCP assigns every byte a:

```text
Sequence Number
```

Purpose:

* Detect Lost Data
* Detect Duplicate Data
* Maintain Correct Order

---

## Example

Suppose sender transmits:

```text
Segment 1
Seq = 1000

Segment 2
Seq = 2000

Segment 3
Seq = 3000
```

Receiver can immediately determine:

```text
Which Segment is Missing
```

or

```text
Which Segment Arrived Out of Order
```

---

# 5. Acknowledgments (ACK)

Receiver confirms successful reception using:

```text
ACK
```

packets.

---

## Example

Sender:

```text
Seq = 1000
```

Receiver replies:

```text
ACK = 2000
```

Meaning:

```text
Everything up to byte 1999
has been received successfully.
```

---

## Why ACK is Needed

Without ACK:

```text
Sender
```

cannot know whether:

```text
Receiver Got the Data
```

or not.

---

# 6. Retransmission

If a segment is lost:

```text
TCP
```

sends it again.

This process is called:

```text
Retransmission
```

---

## Example

```text
Segment 1 ✔

Segment 2 Lost

Segment 3 ✔
```

TCP retransmits:

```text
Segment 2
```

until it is received.

---

# 7. Timeout Mechanism

TCP starts a timer after sending data.

This timer is called:

```text
Retransmission Timer
```

---

## Working

Sender:

```text
Send Segment
      ↓
Start Timer
```

If ACK arrives:

```text
Stop Timer
```

If timer expires:

```text
Retransmit Segment
```

---

## Example

```text
Send Segment
      ↓
Wait 3 Seconds
      ↓
No ACK Received
      ↓
Retransmit
```

---

# 8. Duplicate ACK

Sometimes a packet is lost.

Receiver repeatedly acknowledges the last correctly received segment.

Example:

```text
Segment 1 ✔

Segment 2 Lost

Segment 3 ✔

Segment 4 ✔
```

Receiver sends:

```text
ACK = 2000

ACK = 2000

ACK = 2000
```

These are called:

```text
Duplicate ACKs
```

because the same ACK value is repeated.

---

# 9. Fast Retransmission

TCP does not always wait for timeout.

If:

```text
3 Duplicate ACKs
```

are received,

TCP assumes:

```text
Packet Lost
```

and immediately retransmits the missing segment.

---

## Example

Receiver sends:

```text
ACK=2000

ACK=2000

ACK=2000
```

TCP immediately retransmits:

```text
Missing Segment
```

without waiting for timer expiration.

---

## Advantage

```text
Faster Recovery
```

and

```text
Better Performance
```

---

# 10. TCP Checksum

TCP uses:

```text
Checksum
```

to detect data corruption.

---

## Sender Side

Sender calculates:

```text
Checksum Value
```

and places it in the TCP header.

---

## Receiver Side

Receiver calculates checksum again.

If:

```text
Checksums Match
```

data is accepted.

If:

```text
Checksums Do Not Match
```

segment is discarded.

---

## Purpose

Detect:

* Bit Errors
* Corrupted Data
* Transmission Errors

---

# 11. Sliding Window and Reliability

TCP uses a:

```text
Sliding Window
```

mechanism.

Instead of sending one segment and waiting:

```text
Send

Wait

Send

Wait
```

TCP can send multiple segments before receiving ACKs.

---

## Example

Window Size:

```text
4 Segments
```

Sender may transmit:

```text
Segment 1

Segment 2

Segment 3

Segment 4
```

before receiving acknowledgment.

---

## Benefit

* Better Throughput
* Better Performance
* Reliable Delivery

---

# 12. Summary of TCP Error Control Process

Complete process:

```text
Send Segment
      ↓
Assign Sequence Number
      ↓
Calculate Checksum
      ↓
Transmit Segment
      ↓
Receive ACK?
```

---

If:

```text
YES
```

continue transmission.

---

If:

```text
NO
```

then:

```text
Timeout
      ↓
Retransmission
```

---

If:

```text
3 Duplicate ACKs
```

then:

```text
Fast Retransmission
```

---

# 13. Cybersecurity Perspective

---

## Packet Manipulation

Attackers may attempt:

* Packet Injection
* Packet Modification
* Session Hijacking

TCP Error Control helps detect anomalies.

---

## ACK Flood Attacks

Large numbers of:

```text
ACK Packets
```

may be used to exhaust resources.

---

## TCP Reset Attacks

Attackers may send:

```text
RST Packets
```

to terminate active sessions.

---

## Sequence Number Prediction

If sequence numbers are predictable:

```text
Session Hijacking
```

becomes easier.

Modern TCP implementations use:

```text
Random Initial Sequence Numbers
```

to reduce this risk.

---

## IDS/IPS Monitoring

Security devices frequently inspect:

* Sequence Numbers
* ACK Numbers
* Retransmissions
* Duplicate ACKs
* Checksum Errors

to identify suspicious activity.

---

# 14. Quick Revision Sheet

Error Control Components:

```text
Sequence Numbers

ACKs

Checksum

Timeout

Retransmission

Fast Retransmission
```

---

Purpose of Sequence Numbers:

```text
Ordering

Loss Detection

Duplicate Detection
```

---

Purpose of ACK:

```text
Confirm Delivery
```

---

Purpose of Checksum:

```text
Detect Corruption
```

---

Fast Retransmission Trigger:

```text
3 Duplicate ACKs
```

---

Timeout Action:

```text
Retransmit Segment
```

---

Biggest Concept:

```text
TCP achieves reliable communication
through sequence numbers,
acknowledgments, checksums,
timers, and retransmission mechanisms.
```

---

*End of TCP Error Control Basics Notes*
