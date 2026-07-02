# Computer Networks — DNS Records

> **DNS Records** are entries stored in a DNS server that provide information about a domain name. They tell clients and DNS servers where services are located and how traffic should be handled.
>
> Every time you visit a website, send an email, or connect to an online service, one or more DNS records are queried behind the scenes.

These notes cover:

* What are DNS Records?
* Why DNS Records are Needed
* Types of DNS Records
* A Record
* AAAA Record
* CNAME Record
* MX Record
* NS Record
* SOA Record
* TXT Record
* PTR Record
* SRV Record
* CAA Record
* DNS Record Comparison
* Cybersecurity Perspective
* Quick Revision Sheet

---

# Table of Contents

* [1. What are DNS Records?](#1-what-are-dns-records)
* [2. Why DNS Records are Needed](#2-why-dns-records-are-needed)
* [3. A Record](#3-a-record)
* [4. AAAA Record](#4-aaaa-record)
* [5. CNAME Record](#5-cname-record)
* [6. MX Record](#6-mx-record)
* [7. NS Record](#7-ns-record)
* [8. SOA Record](#8-soa-record)
* [9. TXT Record](#9-txt-record)
* [10. PTR Record](#10-ptr-record)
* [11. SRV Record](#11-srv-record)
* [12. CAA Record](#12-caa-record)
* [13. DNS Record Comparison](#13-dns-record-comparison)
* [14. Cybersecurity Perspective](#14-cybersecurity-perspective)
* [15. Quick Revision Sheet](#15-quick-revision-sheet)

---

# 1. What are DNS Records?

DNS Records are database entries stored on a DNS server.

They contain information that helps convert a domain name into useful network information.

Example:

```text
google.com

↓

DNS Records

↓

IP Address

Mail Server

Name Server

Security Information
```

---

## Example

When you open:

```text
https://google.com
```

DNS first checks:

```text
A Record
```

to obtain Google's IPv4 address.

---

# 2. Why DNS Records are Needed

Different Internet services require different types of information.

Examples:

| Service        | DNS Record |
| -------------- | ---------- |
| Website        | A / AAAA   |
| Email          | MX         |
| DNS Server     | NS         |
| Domain Alias   | CNAME      |
| Reverse Lookup | PTR        |

Without DNS records, DNS servers would not know how to route traffic.

---

# 3. A Record

The **A (Address) Record** maps a domain name to an **IPv4 address**.

---

## Example

```text
example.com

↓

192.168.10.20
```

---

## DIG Command

```bash
dig example.com A
```

---

## Common Uses

* Website Hosting
* API Servers
* Web Applications

---

# 4. AAAA Record

The **AAAA Record** maps a domain name to an **IPv6 address**.

---

## Example

```text
example.com

↓

2001:db8::10
```

---

## DIG Command

```bash
dig example.com AAAA
```

---

## Why AAAA?

Supports modern IPv6 networks.

---

# 5. CNAME Record

**CNAME (Canonical Name)** creates an alias for another hostname.

Instead of pointing directly to an IP address, it points to another domain name.

---

## Example

```text
blog.example.com

↓

example.com

↓

192.168.10.20
```

---

## DIG Command

```bash
dig example.com CNAME
```

---

## Why Use CNAME?

Useful when multiple subdomains should point to the same server.

Example:

```text
www.example.com

shop.example.com

blog.example.com

↓

example.com
```

---

# 6. MX Record

**MX (Mail Exchange)** specifies the mail server responsible for receiving emails.

---

## Example

```text
example.com

↓

mail.example.com
```

---

## DIG Command

```bash
dig example.com MX
```

---

## Priority

MX records have priorities.

Example:

```text
10 mail1.example.com

20 mail2.example.com
```

Lower values have higher priority.

---

# 7. NS Record

**NS (Name Server)** identifies the authoritative DNS servers for a domain.

---

## Example

```text
example.com

↓

ns1.example.com

ns2.example.com
```

---

## DIG Command

```bash
dig example.com NS
```

---

## Why Needed?

Resolvers use NS records to locate the authoritative DNS server.

---

# 8. SOA Record

SOA stands for:

```text
Start of Authority
```

Every DNS zone contains exactly one SOA record.

---

## DIG Command

```bash
dig example.com SOA
```

---

## Information Stored

* Primary Name Server
* Administrator Email
* Serial Number
* Refresh Interval
* Retry Interval
* Expire Time
* Minimum TTL

---

## Why Important?

Secondary DNS servers use the SOA record to determine when updates are available.

---

# 9. TXT Record

TXT records store arbitrary text information.

Today they are commonly used for security and verification.

---

## DIG Command

```bash
dig example.com TXT
```

---

## Common Uses

* SPF
* DKIM
* DMARC
* Domain Verification
* Site Ownership Verification

---

## Example

```text
v=spf1 include:_spf.google.com ~all
```

---

# 10. PTR Record

PTR stands for:

```text
Pointer Record
```

It performs **Reverse DNS Lookup**.

Instead of:

```text
Domain

↓

IP
```

PTR performs:

```text
IP

↓

Domain
```

---

## DIG Command

```bash
dig -x 192.168.1.10
```

---

## Common Uses

* Reverse DNS
* Mail Server Validation
* Log Analysis

---

# 11. SRV Record

SRV stands for:

```text
Service Record
```

It specifies the location of a particular network service.

---

## Example

```text
SIP

LDAP

Kerberos

Microsoft Active Directory
```

---

## Information Stored

* Hostname
* Port Number
* Priority
* Weight

---

## Example

```text
_service._tcp.example.com
```

---

# 12. CAA Record

CAA stands for:

```text
Certification Authority Authorization
```

It specifies which Certificate Authorities (CAs) are allowed to issue SSL/TLS certificates for a domain.

---

## Example

```text
example.com

↓

Let's Encrypt
```

Only the specified CA can issue certificates.

---

## Why Important?

Helps prevent unauthorized SSL certificate issuance.

---

# 13. DNS Record Comparison

| Record | Purpose          | Example            |
| ------ | ---------------- | ------------------ |
| A      | IPv4 Address     | 192.168.1.10       |
| AAAA   | IPv6 Address     | 2001:db8::1        |
| CNAME  | Alias            | blog → example.com |
| MX     | Mail Server      | mail.example.com   |
| NS     | Name Server      | ns1.example.com    |
| SOA    | Zone Information | Primary DNS Server |
| TXT    | Text / Security  | SPF, DKIM          |
| PTR    | Reverse Lookup   | IP → Domain        |
| SRV    | Service Location | LDAP, SIP          |
| CAA    | Allowed CA       | Let's Encrypt      |

---

# 14. Cybersecurity Perspective

DNS records often reveal valuable information during reconnaissance.

---

## Valuable Records

Look for:

* MX
* TXT
* NS
* SOA
* CNAME

These records may reveal infrastructure details.

---

## Information Leakage

TXT records may expose:

* SPF Policies
* Cloud Providers
* Email Services
* Domain Verification Tokens

---

## Mail Infrastructure

MX records reveal:

* Mail Providers
* Email Gateways
* Third-party Email Services

Useful during phishing simulations and security assessments.

---

## Zone Transfer

Misconfigured DNS servers may expose every DNS record through a successful AXFR request.

---

## Reverse Lookup

PTR records help identify:

* Internal Servers
* Mail Servers
* Host Naming Conventions

---

## Important Notes

* Review all available DNS records during reconnaissance.
* Modern DNS servers may restrict some record queries.
* Protect DNS infrastructure against unauthorized zone transfers.
* DNS records often provide valuable intelligence before active scanning.

---

# 15. Quick Revision Sheet

A Record

```bash
dig example.com A
```

---

AAAA Record

```bash
dig example.com AAAA
```

---

MX Record

```bash
dig example.com MX
```

---

NS Record

```bash
dig example.com NS
```

---

TXT Record

```bash
dig example.com TXT
```

---

CNAME Record

```bash
dig example.com CNAME
```

---

SOA Record

```bash
dig example.com SOA
```

---

PTR Record

```bash
dig -x <IP_Address>
```

---

Biggest Concept

```text
DNS Records store different types
of information about a domain.

Each record has a specific purpose,
such as resolving IP addresses,
handling email delivery,
identifying DNS servers,
or providing security-related
information.
```

---

*End of DNS Records Notes*
