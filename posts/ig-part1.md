# Information Gathering for Web Pentesting (Part 1)

> Information Gathering (Reconnaissance) is the first phase of every Web Penetration Test.
>
> The objective is to collect as much information as possible about the target before attempting any vulnerability assessment or exploitation.
>
> DNS enumeration is one of the most important passive and active reconnaissance techniques because it reveals how a domain is configured and often exposes hidden infrastructure.

These notes cover:

* Information Gathering
* DNS Enumeration
* WHOIS
* DNS Records
* DIG Command
* Reverse DNS Lookup
* Using Custom DNS Servers
* DNS Resolution Trace
* DNS Zone Transfer
* Pentesting Perspective
* Quick Revision Sheet

---

# Table of Contents

* [1. Information Gathering](#1-information-gathering)
* [2. DNS Enumeration](#2-dns-enumeration)
* [3. WHOIS](#3-whois)
* [4. DNS Record Types](#4-dns-record-types)
* [5. DIG Command](#5-dig-command)
* [6. Reverse DNS Lookup](#6-reverse-dns-lookup)
* [7. Using a Specific DNS Server](#7-using-a-specific-dns-server)
* [8. DNS Resolution Trace](#8-dns-resolution-trace)
* [9. DNS Zone Transfer](#9-dns-zone-transfer)
* [10. Pentesting Perspective](#10-pentesting-perspective)
* [11. Quick Revision Sheet](#11-quick-revision-sheet)

---

# 1. Information Gathering

Information Gathering (Reconnaissance) is the process of collecting publicly available and technical information about a target.

This phase helps us understand:

* Target Domain
* Public Infrastructure
* DNS Configuration
* Mail Servers
* Web Servers
* Technologies Used
* Subdomains
* Potential Attack Surface

---

## Why Information Gathering?

Proper reconnaissance helps penetration testers:

* Understand the target architecture
* Reduce unnecessary scanning
* Identify hidden assets
* Discover potential attack vectors
* Build an effective testing strategy

---

## Typical Recon Workflow

```text
Target Domain

      │

      ▼

WHOIS Lookup

      │

      ▼

DNS Enumeration

      │

      ▼

Subdomain Enumeration

      │

      ▼

Virtual Host Enumeration

      │

      ▼

Technology Fingerprinting

      │

      ▼

Vulnerability Assessment
```

---

# 2. DNS Enumeration

DNS Enumeration is the process of collecting DNS-related information about a target domain.

DNS may reveal:

* IP Addresses
* Mail Servers
* Name Servers
* Subdomains
* TXT Records
* DNS Misconfigurations

---

## Why DNS Enumeration?

Many organizations expose valuable information through DNS.

For example:

```text
mail.company.com

vpn.company.com

dev.company.com

test.company.com
```

These hosts may later become targets during penetration testing.

---

# 3. WHOIS

WHOIS is a protocol used to retrieve domain registration information.

Syntax:

```bash
whois <domain_name>
```

Example:

```bash
whois example.com
```

---

## Information Obtained

WHOIS may provide:

| Field                  | Description                        |
| ---------------------- | ---------------------------------- |
| Domain Name            | Registered domain                  |
| Registrar              | Company that registered the domain |
| Registrant             | Owner of the domain                |
| Administrative Contact | Administrative manager             |
| Technical Contact      | Technical administrator            |
| Creation Date          | Domain registration date           |
| Expiration Date        | Domain expiry date                 |
| Updated Date           | Last modification date             |
| Name Servers           | DNS servers managing the domain    |

---

## Why is WHOIS Useful?

WHOIS helps identify:

* Domain ownership
* Registration history
* DNS providers
* Organization information
* Infrastructure providers

---

## Example

```bash
whois example.com
```

Example Output:

```text
Registrar:
Example Registrar

Creation Date:
2001-08-10

Name Server:
ns1.example.com

Name Server:
ns2.example.com
```

---

# 4. DNS Record Types

DNS records store different types of information about a domain.

---

## A Record

Maps a domain name to an IPv4 address.

Command:

```bash
dig example.com A
```

Example:

```text
example.com

↓

93.184.216.34
```

---

## AAAA Record

Maps a domain to an IPv6 address.

Command:

```bash
dig example.com AAAA
```

---

## MX Record

Specifies the mail server responsible for handling emails.

Command:

```bash
dig example.com MX
```

Example:

```text
10 mail.example.com
```

---

## NS Record

Identifies the authoritative DNS servers.

Command:

```bash
dig example.com NS
```

Example:

```text
ns1.example.com

ns2.example.com
```

---

## TXT Record

Stores arbitrary text information.

Common uses:

* SPF
* DKIM
* Domain Verification
* DMARC

Command:

```bash
dig example.com TXT
```

---

## CNAME Record

Creates an alias for another hostname.

Example:

```text
blog.example.com

↓

example.com
```

Command:

```bash
dig example.com CNAME
```

---

## SOA Record

SOA stands for:

```text
Start of Authority
```

It contains information about the DNS zone.

Command:

```bash
dig example.com SOA
```

Typical information:

* Primary Name Server
* Administrator Email
* Serial Number
* Refresh Interval
* Retry Interval
* Expire Time

---

# 5. DIG Command

`dig` (Domain Information Groper) is the most commonly used tool for DNS enumeration.

General Syntax:

```bash
dig <domain_name>
```

By default, it performs an **A Record** lookup.

---

## Default Lookup

```bash
dig example.com
```

Equivalent to:

```bash
dig example.com A
```

---

## Common DIG Commands

### IPv4 Address

```bash
dig example.com A
```

---

### IPv6 Address

```bash
dig example.com AAAA
```

---

### Mail Server

```bash
dig example.com MX
```

---

### Name Servers

```bash
dig example.com NS
```

---

### TXT Records

```bash
dig example.com TXT
```

---

### Canonical Name

```bash
dig example.com CNAME
```

---

### Start of Authority

```bash
dig example.com SOA
```

---

### Query All Records

```bash
dig example.com ANY
```

**Important Note**

Many modern DNS servers ignore `ANY` queries to reduce abuse and DNS amplification attacks.

---

# 6. Reverse DNS Lookup

Instead of converting a domain into an IP address, reverse lookup converts an IP address into a hostname.

Syntax:

```bash
dig -x <IP_Address>
```

Example:

```bash
dig -x 192.168.1.1
```

Possible Output:

```text
router.local
```

---

## Why Reverse Lookup?

Useful for:

* Host Identification
* Infrastructure Mapping
* Server Identification
* Internal Reconnaissance

---

# 7. Using a Specific DNS Server

Sometimes we want to query a particular DNS server.

Syntax:

```bash
dig @<DNS_Server> <domain_name>
```

Example:

```bash
dig @1.1.1.1 example.com
```

This queries **Cloudflare DNS** directly.

Other examples:

Google DNS

```bash
dig @8.8.8.8 example.com
```

Quad9 DNS

```bash
dig @9.9.9.9 example.com
```

---

## Why Use Different DNS Servers?

Different DNS servers may:

* Cache different records
* Return different responses
* Help troubleshoot DNS issues

---

# 8. DNS Resolution Trace

To observe the complete DNS resolution process:

```bash
dig +trace example.com
```

---

## How It Works

```text
Root DNS Server

        │

        ▼

Top-Level Domain (.com)

        │

        ▼

Authoritative Name Server

        │

        ▼

Target Domain
```

---

## Why Use +trace?

Useful for:

* Debugging DNS
* Identifying authoritative servers
* Investigating DNS misconfigurations

---

# 9. DNS Zone Transfer

A DNS Zone Transfer copies the complete DNS database from one DNS server to another.

It is normally allowed **only** between trusted name servers.

---

## AXFR Query

Syntax:

```bash
dig axfr @<name_server> <domain_name>
```

Example:

```bash
dig axfr @nsztm1.digi.ninja zonetransfer.me
```

---

## Successful Zone Transfer

If the DNS server is misconfigured, it may reveal:

* All Subdomains
* Internal Hosts
* Mail Servers
* Name Servers
* TXT Records
* Internal Infrastructure

Example:

```text
mail.example.com

vpn.example.com

dev.example.com

test.example.com
```

---

## Why is Zone Transfer Dangerous?

A successful AXFR can disclose an organization's complete DNS structure.

Modern DNS servers usually disable unauthorized zone transfers.

---

# 10. Pentesting Perspective

DNS Enumeration is one of the highest value reconnaissance activities.

---

## Valuable Information to Look For

* Development Servers
* VPN Gateways
* Mail Servers
* Admin Portals
* Backup Servers
* Internal Hostnames

---

## Misconfigurations

Look for:

* Open Zone Transfers
* Information Leakage
* Public Internal Records
* Excessive TXT Records

---

## Useful Commands

WHOIS

```bash
whois example.com
```

DNS Lookup

```bash
dig example.com
```

Reverse Lookup

```bash
dig -x <IP>
```

DNS Trace

```bash
dig +trace example.com
```

Zone Transfer

```bash
dig axfr @<DNS_Server> <domain>
```

---

## Important Notes

* Perform DNS enumeration before active scanning.
* Always verify whether multiple DNS servers return different results.
* Never assume `ANY` queries will work on modern DNS servers.
* A successful zone transfer can significantly expand the attack surface.

---

# 11. Quick Revision Sheet

WHOIS

```bash
whois example.com
```

---

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

Reverse Lookup

```bash
dig -x <IP_Address>
```

---

Specific DNS Server

```bash
dig @1.1.1.1 example.com
```

---

DNS Resolution Trace

```bash
dig +trace example.com
```

---

Zone Transfer

```bash
dig axfr @<DNS_Server> <domain_name>
```

---

Biggest Concept:

```text
DNS Enumeration helps identify
public infrastructure, hidden hosts,
mail servers, DNS configuration,
and potential attack surfaces
before active web penetration testing.
```

---

*End of Information Gathering for Web Pentesting (Part 1)*
