# Computer Networks — Working of VPN

> A **VPN (Virtual Private Network)** creates a secure logical connection over an untrusted public network such as the Internet.
>
> VPNs protect network traffic by creating an encrypted tunnel between the user and a VPN endpoint. They are commonly used for secure Internet access, remote access to private networks, connecting branch offices, and improving privacy on untrusted networks.
>
> A VPN improves privacy and security, but it does not automatically make a user completely anonymous.

These notes cover:

- What is a VPN
- Basic Working of VPN
- VPN Tunnel
- Encryption and Encapsulation
- Virtual Network Interface
- Packet Flow Through a VPN
- Public IP Address and VPN Server
- Accessing Region-Restricted Content
- Remote Access VPN
- Site-to-Site VPN
- VPN vs Leased Line
- Who Needs a VPN
- Limitations of VPN
- Cybersecurity Perspective
- Quick Revision Sheet

---

# Table of Contents

- [1. What is a VPN](#1-what-is-a-vpn)
- [2. Basic Working of VPN](#2-basic-working-of-vpn)
- [3. What is a VPN Tunnel](#3-what-is-a-vpn-tunnel)
- [4. Encryption and Encapsulation](#4-encryption-and-encapsulation)
- [5. Virtual Network Interface](#5-virtual-network-interface)
- [6. Complete Packet Flow Through a VPN](#6-complete-packet-flow-through-a-vpn)
- [7. Public IP Address and VPN Server](#7-public-ip-address-and-vpn-server)
- [8. Accessing Region-Restricted Content](#8-accessing-region-restricted-content)
- [9. Remote Access VPN](#9-remote-access-vpn)
- [10. Site-to-Site VPN](#10-site-to-site-vpn)
- [11. VPN vs Leased Line](#11-vpn-vs-leased-line)
- [12. Who Needs a VPN](#12-who-needs-a-vpn)
- [13. Limitations of VPN](#13-limitations-of-vpn)
- [14. Cybersecurity Perspective](#14-cybersecurity-perspective)
- [15. Quick Revision Sheet](#15-quick-revision-sheet)

---

# 1. What is a VPN

VPN stands for:

```text
Virtual Private Network
```

A VPN creates a:

```text
Secure Logical Connection

over

Public / Untrusted Network
```

such as:

```text
Internet
```

---

## Basic Idea

Normally:

```text
User

↓

ISP

↓

Internet

↓

Destination Server
```

When using a VPN:

```text
User

↓

Encrypted VPN Tunnel

↓

ISP / Public Internet

↓

VPN Server

↓

Internet

↓

Destination Server
```

---

## Main Purposes of VPN

A VPN can provide:

- Confidentiality for traffic crossing untrusted networks
- Secure Remote Access
- Secure Communication between Networks
- Public IP Address Substitution
- Protection on Public Wi-Fi
- Access to Private Organizational Resources

---

# 2. Basic Working of VPN

When a user connects to a VPN, the VPN client first establishes a connection with the VPN server.

```text
User Device

↓

VPN Client Starts

↓

VPN Server Connection Established

↓

VPN Authentication

↓

Cryptographic Keys Established

↓

VPN Tunnel Created

↓

Traffic Routed Through VPN Tunnel
```

After the tunnel is established:

```text
Application Generates Data

↓

Operating System Creates Packets

↓

VPN Software Receives Packets

↓

Packets Protected and Encapsulated

↓

Traffic Sent Through Internet

↓

VPN Server Receives Traffic

↓

Outer VPN Protection Removed

↓

Original Packet Forwarded
Toward Destination
```

The response follows the reverse path.

---

## Important Concept

The ISP can normally see that the user is communicating with a VPN server.

However, when a properly configured encrypted VPN protocol is used:

```text
ISP

can see

User ↔ VPN Server Communication
```

but generally cannot directly read:

```text
Protected Traffic Inside
the VPN Tunnel
```

---

# 3. What is a VPN Tunnel

A VPN tunnel is a:

```text
Logical Communication Path

created over

Public Network
```

It is not a physical cable.

It is created using:

```text
VPN Protocol

+

Cryptographic Protection

+

Encapsulation
```

---

## Basic Tunnel

```text
User Device

==============================

       VPN Tunnel

==============================

VPN Server
```

The Internet carries the VPN packets, but the protected inner traffic travels logically through the tunnel.

---

## Important Concept

```text
Public Internet

provides

Physical Network Connectivity
```

while:

```text
VPN

creates

Secure Logical Connectivity
```

over that network.

---

# 4. Encryption and Encapsulation

Two important concepts in VPN communication are:

```text
Encryption

and

Encapsulation
```

They are related but different.

---

## Encryption

Encryption converts readable data into protected ciphertext.

```text
Original Data

↓

Encryption

↓

Encrypted Data
```

Without the correct cryptographic key, the protected content should not be readable.

---

## Encapsulation

Encapsulation places the original packet inside another packet used by the VPN protocol.

Conceptually:

```text
Original Packet

↓

Protected by VPN

↓

Placed Inside
Outer VPN Packet

↓

Transmitted Through Internet
```

---

## Before VPN Processing

Suppose a web application generates traffic.

```text
Application Data

↓

TCP / UDP Segment

↓

IP Packet
```

Conceptually:

```text
+-----------------------------+
| Original IP Header          |
+-----------------------------+
| TCP / UDP Header            |
+-----------------------------+
| Application Data            |
+-----------------------------+
```

---

## After VPN Processing

The exact packet format depends on the VPN protocol.

Conceptually:

```text
+-----------------------------+
| New Outer IP Header         |
+-----------------------------+
| VPN Protocol Information    |
+-----------------------------+
| Protected Inner Packet      |
+-----------------------------+
```

The protected inner packet contains the original network traffic.

---

## Important Concept

```text
Original Packet

↓

VPN Processing

↓

Encryption / Authentication

↓

Encapsulation

↓

Outer VPN Packet

↓

Public Internet
```

---

# 5. Virtual Network Interface

Many VPN implementations create a virtual network interface.

Examples on Linux may include:

```text
tun0
```

or:

```text
wg0
```

depending on the VPN software and protocol.

---

## Physical Interface

Examples:

```text
eth0

wlan0
```

These represent actual physical network interfaces.

---

## Virtual Interface

Example:

```text
tun0
```

This is a software-created network interface.

It allows the operating system to route selected traffic toward the VPN software.

---

## Basic Working

```text
Application

↓

Operating System
Network Stack

↓

Routing Table

↓

Virtual VPN Interface

↓

VPN Software

↓

Protect + Encapsulate Packet

↓

Physical Interface

↓

Internet
```

---

## Important Concept

The virtual interface does not directly transmit packets through a physical cable.

Instead:

```text
Virtual Interface

↓

Passes Packets to
VPN Software

↓

VPN Software Creates
Outer VPN Traffic

↓

Physical Interface
Transmits Traffic
```

---

# 6. Complete Packet Flow Through a VPN

Suppose a user opens:

```text
example.com
```

while connected to a full-tunnel VPN.

---

## Step 1: Application Generates Traffic

```text
Browser

↓

Application Data
```

---

## Step 2: Operating System Creates Network Traffic

```text
Application Data

↓

TCP / UDP

↓

IP Packet
```

---

## Step 3: Routing Decision

The operating system checks its routing table.

Because the VPN is configured as the route for this traffic:

```text
Original Packet

↓

Virtual VPN Interface
```

---

## Step 4: VPN Client Processes the Packet

The VPN software receives the original packet.

```text
Original Packet

↓

Encryption / Authentication

↓

Encapsulation
```

The exact operations depend on the VPN protocol.

---

## Step 5: Outer Packet is Created

Conceptually:

```text
Outer Source IP

=

User's Network Public IP


Outer Destination IP

=

VPN Server IP
```

The ISP forwards this outer packet toward the VPN server.

---

## Step 6: Packet Travels Through Public Internet

```text
User Device

↓

Router

↓

ISP

↓

Public Internet

↓

VPN Server
```

The ISP can observe the outer VPN communication.

The protected inner packet content is not directly visible when strong encryption is correctly used.

---

## Step 7: VPN Server Receives the Packet

```text
VPN Server

↓

Receives Outer Packet

↓

Verifies / Decrypts VPN Traffic

↓

Removes VPN Encapsulation

↓

Recovers Original Packet
```

---

## Step 8: VPN Server Forwards Traffic

The VPN server sends the traffic toward the destination.

```text
VPN Server

↓

Internet

↓

Destination Server
```

In a typical consumer VPN setup, the VPN server performs address translation so the destination sees the VPN server's public IP address.

---

## Step 9: Destination Sends Response

```text
Destination Server

↓

VPN Server
```

The response returns to the VPN server.

---

## Step 10: Response Sent Back Through Tunnel

```text
VPN Server

↓

Protects and Encapsulates Response

↓

Public Internet

↓

User Device

↓

VPN Client Decrypts Traffic

↓

Original Response Delivered
to Application
```

---

## Complete Flow

```text
Application

↓

Original IP Packet

↓

Virtual VPN Interface

↓

VPN Client

↓

Encryption / Authentication

↓

Encapsulation

↓

Physical Interface

↓

ISP

↓

Public Internet

↓

VPN Server

↓

Decryption / Decapsulation

↓

Destination Website

↓

Response

↓

VPN Server

↓

Encrypted VPN Tunnel

↓

User Device

↓

Application
```

---

# 7. Public IP Address and VPN Server

Without a VPN:

```text
User

↓

ISP

↓

Destination Website
```

The website normally sees the public IP address used by the user's Internet connection.

---

With a typical consumer VPN:

```text
User

↓

Encrypted VPN Tunnel

↓

VPN Server

↓

Destination Website
```

The website normally sees:

```text
VPN Server's Public IP Address
```

instead of:

```text
User's ISP-Assigned Public IP Address
```

---

## Important Concept

A VPN does not change the public IP address assigned to the user's Internet connection.

Instead:

```text
User's Real Network Connection

↓

Connects to VPN Server

↓

VPN Server Forwards Traffic

↓

Destination Sees
VPN Server Public IP
```

---

# 8. Accessing Region-Restricted Content

Some services make access decisions based on the apparent public IP address and its geolocation.

Suppose:

```text
User Public IP

↓

Region A
```

and a service is unavailable in that region.

Without VPN:

```text
User

↓

ISP

↓

Destination Service

↓

User's Public IP Observed

↓

Access Decision
```

---

If the user connects to a VPN server in another region:

```text
User

↓

Encrypted VPN Tunnel

↓

VPN Server in Region B

↓

Destination Service
```

The destination generally sees:

```text
VPN Server's Public IP
```

and may associate the connection with:

```text
Region B
```

---

## Important Note

Using a VPN does not guarantee access to region-restricted services.

Services may:

- Detect VPN IP Addresses
- Block Datacenter Networks
- Require Account Region Verification
- Use Additional Location Signals
- Restrict VPN Usage in Their Terms of Service

---

# 9. Remote Access VPN

A Remote Access VPN allows an individual user to securely connect to a private network from another location.

Example:

```text
Employee at Home

↓

Public Internet

↓

Encrypted VPN Tunnel

↓

Company VPN Gateway

↓

Private Company Network
```

---

## Working

```text
Remote User

↓

VPN Client

↓

Authentication

↓

VPN Tunnel Established

↓

Public Internet

↓

Company VPN Gateway

↓

Private Network Resources
```

---

## Use Cases

- Work From Home
- Remote Administration
- Access Internal Applications
- Access Private Servers
- Secure Communication over Untrusted Networks

---

# 10. Site-to-Site VPN

A Site-to-Site VPN connects two or more networks securely over the Internet.

Example:

```text
Office A Network

↓

VPN Gateway

=======================

     VPN Tunnel

=======================

VPN Gateway

↓

Office B Network
```

---

## Main Purpose

```text
Connect Private Networks

over

Public Internet
```

---

## Example

A company has offices in:

```text
Kolkata

and

Bengaluru
```

Instead of using a dedicated physical private connection:

```text
Office A

↓

VPN Gateway

↓

Public Internet

↓

VPN Gateway

↓

Office B
```

The gateways create a secure tunnel between the two private networks.

---

# 11. VPN vs Leased Line

A leased line is a dedicated communication connection provided by a service provider.

A VPN creates secure logical connectivity using an existing network such as the Internet.

---

| Feature | Leased Line | VPN |
|---------|-------------|-----|
| Connection | Dedicated Provider Circuit | Logical Tunnel over Existing Network |
| Infrastructure | Private / Dedicated Connectivity | Uses Public or Shared Infrastructure |
| Cost | Generally Higher | Generally Lower |
| Performance | More Predictable | Depends on Internet Connection |
| Security | Dedicated Connectivity, but Encryption May Still Be Needed | Commonly Uses Encryption and Authentication |
| Deployment | Requires Provider Infrastructure | Usually Easier to Deploy |
| Common Usage | Critical Business Connectivity | Remote Access and Network Connectivity |

---

## Basic Concept

```text
Leased Line

↓

Dedicated Connectivity

↓

Higher Cost
```

```text
VPN

↓

Secure Logical Tunnel

↓

Existing Internet Connection

↓

Lower Connectivity Cost
```

---

# 12. Who Needs a VPN

VPNs are commonly useful for:

---

## Remote Employees

```text
Remote Employee

↓

VPN

↓

Company Network
```

Provides secure access to internal organizational resources.

---

## Organizations

Organizations can connect:

```text
Branch Office

↓

VPN Tunnel

↓

Head Office
```

---

## Public Wi-Fi Users

A VPN can protect traffic between the device and VPN server when using untrusted Wi-Fi networks.

```text
User

↓

Untrusted Wi-Fi

↓

Encrypted VPN Tunnel

↓

VPN Server
```

---

## Users Seeking Better Network Privacy

Without VPN:

```text
ISP

can observe destination
network metadata and
unencrypted traffic
```

With VPN:

```text
ISP

primarily observes

Encrypted Connection
to VPN Server
```

However:

```text
VPN Provider

becomes an important
trust point.
```

---

# 13. Limitations of VPN

A VPN provides important security and privacy benefits, but it does not solve every security problem.

---

## VPN Does Not Provide Complete Anonymity

Websites may still identify users through:

- Account Logins
- Browser Cookies
- Browser Fingerprinting
- Tracking Technologies
- Device Identifiers
- Application-Level Information

---

## VPN Provider Must Be Trusted

Traffic leaves the encrypted tunnel at the VPN server.

```text
User

↓

Encrypted Tunnel

↓

VPN Provider

↓

Internet
```

Therefore:

```text
Trust Moves

from

Local ISP

toward

VPN Provider
```

---

## VPN Cannot Protect a Compromised Device

If the user's system contains:

```text
Malware

Spyware

Credential Stealer
```

a VPN cannot prevent the malware from stealing information directly from the device.

---

## VPN May Reduce Performance

VPN processing may introduce:

```text
Encryption Overhead

+

Encapsulation Overhead

+

Longer Network Path

↓

Higher Latency

and

Possible Speed Reduction
```

---

## VPN Does Not Replace HTTPS

VPN and HTTPS protect different parts of communication.

```text
VPN

Protects Traffic

User Device ↔ VPN Server
```

```text
HTTPS

Protects Application Communication

Client ↔ HTTPS Server
```

Using HTTPS remains important even when connected to a VPN.

---

# 14. Cybersecurity Perspective

---

## Protection on Untrusted Networks

A VPN can protect traffic from local network observers when using:

```text
Public Wi-Fi

Hotel Networks

Airport Networks

Other Untrusted Networks
```

---

## VPN Concentrator as a High-Value Target

Corporate VPN gateways are exposed network services.

Attackers may target:

- Weak Credentials
- Stolen Credentials
- Unpatched VPN Appliances
- Misconfigurations
- Weak Authentication
- Exposed Management Interfaces

---

## Credential Attacks

Internet-facing VPN services may experience:

```text
Password Spraying

Brute-Force Attempts

Credential Stuffing
```

Organizations should use:

```text
Multi-Factor Authentication

Strong Password Policies

Rate Limiting

Authentication Monitoring
```

---

## VPN Traffic Monitoring

Security teams may monitor:

- Unusual Login Locations
- Impossible Travel Events
- Repeated Authentication Failures
- Connections from Suspicious IP Addresses
- Unusual Session Duration
- Unexpected Data Transfer
- Connections from Unauthorized Devices

---

## Split Tunneling Risk

In a split-tunnel configuration:

```text
Corporate Traffic

↓

VPN Tunnel
```

while:

```text
Other Internet Traffic

↓

Direct Internet Connection
```

Split tunneling can improve performance but may increase security risk if the endpoint simultaneously communicates with:

```text
Private Corporate Network

and

Untrusted Internet Resources
```

---

## VPN Does Not Guarantee Anonymity

A VPN primarily hides the user's ISP-assigned public IP address from the destination server.

It does not automatically hide:

```text
Identity

Browser Fingerprint

Account Information

Cookies

Device Information

Application Metadata
```

---

## Important Security Concept

```text
VPN

is a

Secure Communication Technology

not a

Complete Anonymity Solution
```

---

# 15. Quick Revision Sheet

VPN

```text
Virtual Private Network
```

Purpose:

```text
Create Secure Logical Connectivity

over

Public / Untrusted Network
```

---

Basic Working:

```text
User

↓

VPN Client

↓

VPN Tunnel Established

↓

Traffic Protected

↓

Packet Encapsulated

↓

ISP / Public Internet

↓

VPN Server

↓

Decryption / Decapsulation

↓

Destination
```

---

VPN Tunnel:

```text
Logical Communication Path

created over

Public Network
```

---

Encryption:

```text
Readable Data

↓

Encryption

↓

Protected Ciphertext
```

---

Encapsulation:

```text
Original Packet

↓

Placed Inside

Outer VPN Packet
```

---

Virtual Interface:

```text
Application

↓

OS Network Stack

↓

Routing Table

↓

tun0 / wg0

↓

VPN Software

↓

Physical Interface

↓

Internet
```

---

Public IP Concept:

```text
Without VPN

Destination Sees

User's Public IP
```

```text
With Typical Consumer VPN

Destination Sees

VPN Server's Public IP
```

---

Remote Access VPN:

```text
Remote User

↓

VPN Tunnel

↓

Organization's Private Network
```

---

Site-to-Site VPN:

```text
Private Network A

↓

VPN Gateway

↓

Internet

↓

VPN Gateway

↓

Private Network B
```

---

VPN vs Leased Line:

```text
Leased Line

↓

Dedicated Connectivity

↓

Generally Higher Cost
```

```text
VPN

↓

Secure Logical Tunnel

↓

Uses Existing Internet Connectivity

↓

Generally Lower Cost
```

---

Security Limitation:

```text
VPN Does Not Automatically Provide

Complete Anonymity

Malware Protection

Protection from Browser Tracking

Protection from Account-Based Identification
```

---

Biggest Concept:

```text
A VPN creates secure logical
connectivity over a public or
untrusted network.

The operating system routes selected
traffic to the VPN software, often
through a virtual network interface.

The VPN protects the original traffic
and encapsulates it inside outer
packets that travel through the
public Internet to the VPN server.

The VPN server removes the VPN
protection and forwards the traffic
toward the destination.

In a typical consumer VPN setup,
the destination sees the VPN
server's public IP address instead
of the user's ISP-assigned public IP.

VPNs are also used for remote access
and site-to-site connectivity.

A VPN improves security and privacy,
but it does not provide complete
anonymity or replace endpoint security
and HTTPS.
```

---

*End of Working of VPN Notes*