# Computer Networks — SMTP, POP3 & IMAP

> **SMTP, POP3, and IMAP** are important Application Layer protocols used in email communication.
>
> **SMTP (Simple Mail Transfer Protocol)** is used to send and transfer emails, while **POP3 (Post Office Protocol Version 3)** and **IMAP (Internet Message Access Protocol)** are used by email clients to access received emails from a mail server.
>
> Understanding these protocols helps us understand how email is sent between mail servers and how users access their mailboxes.

These notes cover:

- SMTP
- How SMTP Works
- SMTP Ports
- POP3
- How POP3 Works
- POP3 Ports
- IMAP
- How IMAP Works
- IMAP Ports
- POP3 vs IMAP
- SMTP vs POP3 vs IMAP
- Gmail to Outlook Email Delivery
- Cybersecurity Perspective
- Quick Revision Sheet

---

# Table of Contents

- [1. SMTP](#1-smtp)
- [2. How SMTP Works](#2-how-smtp-works)
- [3. POP3](#3-pop3)
- [4. How POP3 Works](#4-how-pop3-works)
- [5. IMAP](#5-imap)
- [6. How IMAP Works](#6-how-imap-works)
- [7. POP3 vs IMAP](#7-pop3-vs-imap)
- [8. SMTP vs POP3 vs IMAP](#8-smtp-vs-pop3-vs-imap)
- [9. Gmail to Outlook Email Delivery](#9-gmail-to-outlook-email-delivery)
- [10. Cybersecurity Perspective](#10-cybersecurity-perspective)
- [11. Quick Revision Sheet](#11-quick-revision-sheet)

---

# 1. SMTP

SMTP stands for:

```text
Simple Mail Transfer Protocol
```

SMTP is an **Application Layer protocol** used to send and transfer email messages.

SMTP is used between:

```text
Email Client

↓

Sending Mail Server
```

and:

```text
Sending Mail Server

↓

Receiving Mail Server
```

---

## Main Purpose of SMTP

SMTP is responsible for:

```text
Sending Email

+

Transferring Email
Between Mail Servers
```

SMTP is mainly a:

```text
Push Protocol
```

The sender pushes the email toward the destination mail server.

---

## SMTP Ports

| Port | Purpose |
|------|---------|
| 25 | Mail Server to Mail Server Transfer |
| 465 | SMTP Submission with Implicit TLS |
| 587 | Email Submission |

---

## Important Concept

```text
SMTP

↓

SEND EMAIL
```

SMTP does not retrieve emails from the user's mailbox.

---

# 2. How SMTP Works

Suppose:

```text
alice@example.com
```

sends an email to:

```text
bob@example.net
```

---

## Working

```text
Alice Writes Email

↓

Email Submitted to
Sending Mail Server

↓

Recipient Domain Identified

example.net

↓

DNS MX Record Lookup

↓

Destination Mail Server Found

↓

SMTP Connection Established

↓

Email Transferred

↓

Receiving Mail Server
Accepts Email

↓

Email Stored in
Bob's Mailbox
```

---

## Role of DNS

The sending mail server must find the mail server responsible for the recipient's domain.

For this, it performs an:

```text
MX Record Lookup
```

Example:

```text
example.net

↓

MX Record

↓

mail.example.net
```

The destination mail server hostname is then resolved using:

```text
A Record

or

AAAA Record
```

Working:

```text
mail.example.net

↓

A / AAAA Record

↓

IP Address

↓

SMTP Connection
```

---

# 3. POP3

POP3 stands for:

```text
Post Office Protocol Version 3
```

POP3 is an **Application Layer protocol** used by an email client to retrieve emails from a mail server.

---

## Basic Communication

```text
Mail Server

↓

POP3

↓

Email Client
```

---

## Main Purpose

```text
Download Emails

from

Mail Server
```

POP3 traditionally follows a:

```text
Download-and-Delete Model
```

Working:

```text
Connect to Mail Server

↓

Download Emails

↓

Store Emails Locally

↓

Optionally Delete Emails
from Server
```

Modern email clients can also be configured to:

```text
Leave a Copy on Server
```

---

## POP3 Ports

| Port | Purpose |
|------|---------|
| 110 | POP3 |
| 995 | POP3 over Implicit TLS |

---

## Important Concept

```text
POP3

↓

DOWNLOAD EMAIL
```

POP3 is mainly useful when emails need to be downloaded and stored on a local device.

---

# 4. How POP3 Works

Suppose emails are already stored inside the user's mailbox.

The email client connects to the POP3 server.

---

## Working

```text
Email Stored
on Mail Server

↓

Email Client Connects
to POP3 Server

↓

User Authentication

↓

Mailbox Access

↓

Available Emails Listed

↓

Emails Downloaded

↓

Emails Stored Locally

↓

Optionally Delete Emails
from Server

↓

Connection Closed
```

---

## POP3 Working States

POP3 works using three main states.

```text
Authorization

↓

Transaction

↓

Update
```

---

### Authorization

The user connects and authenticates with the mail server.

```text
Client

↓

Authentication

↓

Mailbox Access
```

---

### Transaction

The client performs mailbox operations.

Examples:

```text
View Message List

Download Messages

Mark Messages
for Deletion
```

---

### Update

The client closes the connection.

The server processes requested changes.

Example:

```text
Messages Marked
for Deletion

↓

Connection Closed

↓

Messages Deleted
```

---

## Limitation of POP3

POP3 mainly downloads emails to the local device.

Therefore:

```text
Changes Made on One Device

may not automatically

Synchronize with Other Devices
```

For users accessing the same mailbox from multiple devices:

```text
IMAP is generally preferred.
```

---

# 5. IMAP

IMAP stands for:

```text
Internet Message Access Protocol
```

IMAP is an **Application Layer protocol** used to access and manage emails stored on a mail server.

Unlike POP3, IMAP keeps the main mailbox on the server.

---

## Basic Communication

```text
Email Client

↕

IMAP

↕

Mail Server
```

---

## Main Purpose

```text
Access Email

+

Manage Email

+

Synchronize Mailbox
Across Multiple Devices
```

---

## Important Concept

IMAP follows a:

```text
Server-Based
Synchronization Model
```

Emails remain stored on the mail server.

The email client synchronizes mailbox information with the server.

---

## Example

Suppose the user has:

```text
Laptop

Phone

Tablet
```

All devices access the same mailbox.

```text
                Mail Server

                     │

          ┌──────────┼──────────┐

          │          │          │

          ▼          ▼          ▼

       Laptop      Phone      Tablet
```

If the user reads an email on the phone:

```text
Email Marked as Read

↓

Mail Server Updated

↓

Laptop Synchronizes

↓

Tablet Synchronizes

↓

Email Appears Read
on All Devices
```

---

## IMAP Ports

| Port | Purpose |
|------|---------|
| 143 | IMAP |
| 993 | IMAP over Implicit TLS |

---

## Important Concept

```text
IMAP

↓

ACCESS

+

MANAGE

+

SYNCHRONIZE EMAIL
```

---

# 6. How IMAP Works

Suppose emails are stored in the user's mailbox.

---

## Working

```text
Email Stored
on Mail Server

↓

Email Client Connects
to IMAP Server

↓

User Authentication

↓

Mailbox Information
Synchronized

↓

User Views Emails

↓

User Reads / Deletes /
Moves an Email

↓

Changes Sent
to Mail Server

↓

Mail Server Updates Mailbox

↓

Other Devices Synchronize
the Same Changes
```

---

## Example

User reads an email using a smartphone.

```text
Phone

↓

Email Opened

↓

Mail Server Updated

↓

Message Marked as Read
```

Later the user opens the mailbox on a laptop.

```text
Laptop

↓

Connects to Mail Server

↓

Mailbox Synchronized

↓

Email Already Appears Read
```

---

## What IMAP Synchronizes

IMAP can synchronize:

- Emails
- Read / Unread Status
- Mail Folders
- Deleted Messages
- Moved Messages
- Message Flags

---

## Why IMAP is Commonly Used

Modern users access email from:

```text
Phone

Laptop

Tablet

Webmail
```

IMAP keeps mailbox information synchronized across these devices.

---

# 7. POP3 vs IMAP

| Feature | POP3 | IMAP |
|---------|------|------|
| Full Form | Post Office Protocol Version 3 | Internet Message Access Protocol |
| Main Purpose | Download Email | Access and Synchronize Email |
| Email Storage | Mainly Local Device | Mail Server |
| Multiple Devices | Limited Synchronization | Designed for Multiple Devices |
| Folder Synchronization | No | Yes |
| Read / Unread Synchronization | No | Yes |
| Internet Requirement | Emails Can Be Read Locally After Download | Usually Synchronizes with Server |
| Common Ports | 110, 995 | 143, 993 |

---

## Simple Concept

```text
POP3

↓

Download Email

↓

Store Locally
```

```text
IMAP

↓

Keep Email on Server

↓

Synchronize Across Devices
```

---

# 8. SMTP vs POP3 vs IMAP

| Feature | SMTP | POP3 | IMAP |
|---------|------|------|------|
| Full Form | Simple Mail Transfer Protocol | Post Office Protocol Version 3 | Internet Message Access Protocol |
| Purpose | Send and Transfer Email | Download Email | Access and Synchronize Email |
| Protocol Type | Push-Oriented | Retrieval Protocol | Mailbox Access Protocol |
| Direction | Client → Server / Server → Server | Server → Client | Client ↔ Server |
| Email Stored Mainly | Destination Mail Server | Local Device after Download | Mail Server |
| Multiple Device Support | Not Applicable | Limited | Good |
| Common Ports | 25, 465, 587 | 110, 995 | 143, 993 |

---

## Easy Concept

```text
SMTP

↓

SEND EMAIL
```

```text
POP3

↓

DOWNLOAD EMAIL
```

```text
IMAP

↓

ACCESS AND
SYNCHRONIZE EMAIL
```

---

# 9. Gmail to Outlook Email Delivery

Suppose:

```text
Sender:

alice@gmail.com
```

sends an email to:

```text
Receiver:

bob@outlook.com
```

---

## Complete Working

```text
Alice Writes Email
Using Gmail

↓

Alice Clicks Send

↓

Gmail Accepts Email

↓

Gmail Identifies
Recipient Domain

outlook.com

↓

Gmail Performs
DNS MX Record Lookup

↓

Microsoft Mail Server Found

↓

Mail Server Hostname
Resolved to IP Address

↓

Gmail Mail Infrastructure
Connects to Microsoft
Mail Infrastructure

↓

SMTP Transfers Email

↓

Microsoft Mail Server
Receives Email

↓

Spam and Security
Checks Performed

↓

Email Stored in
Bob's Mailbox

↓

Bob Accesses Email
Using an Email Client

↓

POP3

or

IMAP

↓

Bob Reads Email
```

---

## If Bob Uses POP3

```text
Email Stored
on Microsoft Mail Server

↓

Outlook Email Client
Connects Using POP3

↓

Email Downloaded

↓

Email Stored Locally

↓

Email May Remain on Server
or Be Deleted Depending
on Client Configuration
```

---

## If Bob Uses IMAP

```text
Email Stored
on Microsoft Mail Server

↓

Email Client Connects
Using IMAP

↓

Mailbox Synchronized

↓

Bob Reads Email

↓

Read Status Updated
on Mail Server

↓

Other Devices Synchronize
the Same Change
```

---

## Role of Each Protocol

### DNS

```text
Find Destination
Mail Server
```

---

### SMTP

```text
Send and Transfer Email
```

---

### POP3

```text
Download Email
from Mail Server
```

---

### IMAP

```text
Access and Synchronize
Email with Mail Server
```

---

## Complete Email Flow

```text
Sender

↓

SMTP

↓

Sending Mail Server

↓

DNS MX Lookup

↓

Receiving Mail Server

↓

Recipient Mailbox

↓

POP3 / IMAP

↓

Recipient Email Client

↓

Recipient Reads Email
```

---

# 10. Cybersecurity Perspective

---

## SMTP Security

SMTP is an important protocol during email security assessments.

Common security concerns include:

- Email Spoofing
- Open Mail Relay
- Spam
- Phishing
- Malware Delivery
- Weak TLS Configuration

---

## Email Authentication

Modern email systems use:

```text
SPF

DKIM

DMARC
```

These technologies help reduce:

```text
Email Spoofing

and

Unauthorized Domain Usage
```

---

## POP3 Security

Unencrypted POP3 communication may expose:

```text
Username

Password

Email Content
```

Secure POP3 communication should use encryption.

```text
POP3S

↓

Port 995
```

---

## IMAP Security

Unencrypted IMAP communication may also expose:

```text
Authentication Credentials

Email Content

Mailbox Information
```

Secure IMAP communication commonly uses:

```text
IMAPS

↓

Port 993
```

---

## Credential Attacks

Exposed mail services may be targeted using:

```text
Password Spraying

Credential Stuffing

Brute-Force Attempts
```

Strong authentication and rate limiting are important security controls.

---

## Mail Server Enumeration

During authorized security assessments, exposed mail services may reveal:

- Mail Server Software
- Supported Protocols
- Authentication Methods
- TLS Configuration
- Mail Infrastructure

---

## Important Notes

- SMTP is used to send and transfer emails.
- POP3 is used to download emails from a mail server.
- IMAP is used to access and synchronize emails stored on a mail server.
- Prefer encrypted mail protocols.
- Use strong authentication for email accounts.
- Monitor mail servers for unusual authentication attempts.
- SPF, DKIM, and DMARC help protect domains against email spoofing.

---

# 11. Quick Revision Sheet

SMTP

```text
Simple Mail Transfer Protocol
```

Purpose:

```text
Send and Transfer Email
```

Ports:

```text
25  → Server-to-Server Transfer

465 → SMTP Submission
      with Implicit TLS

587 → Email Submission
```

---

POP3

```text
Post Office Protocol Version 3
```

Purpose:

```text
Download Email
from Mail Server
```

Ports:

```text
110 → POP3

995 → POP3 over
      Implicit TLS
```

---

IMAP

```text
Internet Message Access Protocol
```

Purpose:

```text
Access

Manage

Synchronize Email
```

Ports:

```text
143 → IMAP

993 → IMAP over
      Implicit TLS
```

---

POP3 vs IMAP

```text
POP3

↓

Download Email

↓

Store Locally
```

```text
IMAP

↓

Keep Mailbox on Server

↓

Synchronize Across Devices
```

---

Complete Email Working

```text
Sender Writes Email

↓

SMTP Sends Email

↓

Sending Mail Server

↓

DNS MX Lookup

↓

Receiving Mail Server

↓

Email Stored in Mailbox

↓

POP3 / IMAP

↓

Recipient Accesses Email
```

---

Biggest Concept

```text
SMTP is used to send and
transfer emails.

DNS MX records help the sending
mail server locate the receiving
mail server.

POP3 downloads emails from the
mail server to an email client.

IMAP keeps the mailbox on the
server and synchronizes emails
and mailbox changes across
multiple devices.

When Gmail sends an email to
Outlook, SMTP transfers the email
between the mail infrastructures,
while the recipient can access
the received email using POP3,
IMAP, or webmail.
```

---

*End of SMTP, POP3 & IMAP Notes*