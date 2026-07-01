# Computer Networks — DNS and Its Working

> **DNS (Domain Name System)** is the Internet's naming system that translates human-readable domain names (such as **google.com**) into IP addresses (such as **142.250.183.110**).
>
> Without DNS, users would have to remember IP addresses instead of easy-to-read domain names. DNS acts like the **phonebook of the Internet**, allowing users to access websites using names rather than numbers.

These notes cover:

* What is DNS?
* Why DNS is Needed
* Domain Name Structure
* DNS Hierarchy
* DNS Components
* DNS Resolution Process
* Recursive & Iterative Queries
* DNS Caching
* Common DNS Records
* DNS Ports
* Cybersecurity Perspective
* Quick Revision Sheet

---

# Table of Contents

* [1. What is DNS?](#1-what-is-dns)
* [2. Why DNS is Needed](#2-why-dns-is-needed)
* [3. Domain Name Structure](#3-domain-name-structure)
* [4. DNS Hierarchy](#4-dns-hierarchy)
* [5. DNS Components](#5-dns-components)
* [6. DNS Resolution Process](#6-dns-resolution-process)
* [7. Recursive and Iterative Queries](#7-recursive-and-iterative-queries)
* [8. DNS Caching](#8-dns-caching)
* [9. Common DNS Records](#9-common-dns-records)
* [10. DNS Ports](#10-dns-ports)
* [11. Cybersecurity Perspective](#11-cybersecurity-perspective)
* [12. Quick Revision Sheet](#12-quick-revision-sheet)

---

# 1. What is DNS?

DNS stands for:

```text
Domain Name System
```

Its primary job is to convert a **domain name** into an **IP address**.

Example:

```text
google.com

        ↓

142.250.xxx.xxx
```

Your browser communicates using IP addresses, not domain names.

---

## Example

Instead of remembering:

```text
142.250.183.110
```

Users simply type:

```text
google.com
```

DNS performs the translation automatically.

---

# 2. Why DNS is Needed

Computers communicate using IP addresses.

Humans prefer using names.

DNS acts as a translator between them.

---

## Without DNS

```text
User

↓

142.250.183.110
```

Difficult to remember.

---

## With DNS

```text
User

↓

google.com

↓

DNS

↓

142.250.183.110
```

Much easier for users.

---

## Advantages of DNS

* Easy to remember
* Faster website access
* Scalable
* Distributed architecture
* Supports load balancing
* Allows domain name changes without affecting users

---

# 3. Domain Name Structure

A domain name consists of multiple parts.

Example:

```text
www.google.com
```

---

## Structure

```text
www.google.com

│     │      │

│     │      └── Top Level Domain (TLD)

│     └──────── Second Level Domain

└────────────── Subdomain / Hostname
```

---

## Components

### Top Level Domain (TLD)

Examples:

```text
.com

.org

.net

.edu

.gov
```

---

### Second Level Domain

The organization or company name.

Example:

```text
google
```

---

### Subdomain

Used to organize services.

Examples:

```text
mail.google.com

docs.google.com

drive.google.com
```

---

# 4. DNS Hierarchy

DNS is organized as a hierarchical distributed database.

```text
                 Root (.)

                    │

        ┌───────────┴───────────┐

       .com        .org        .net

         │

      google

         │

       www
```

---

## Levels

1. Root Server
2. Top Level Domain (TLD)
3. Authoritative Name Server
4. Domain
5. Subdomain

---

# 5. DNS Components

Several DNS servers work together during name resolution.

---

## Client

The user's computer requesting a domain lookup.

Example:

```text
Laptop

↓

google.com
```

---

## Recursive Resolver

Usually provided by:

* ISP
* Google DNS
* Cloudflare DNS

It performs the lookup on behalf of the client.

Examples:

```text
8.8.8.8

1.1.1.1
```

---

## Root Name Server

The first server contacted during DNS resolution.

It does **not** know the IP address.

Instead, it tells the resolver which TLD server to ask.

---

## TLD Name Server

Responsible for domains such as:

```text
.com

.org

.net
```

It points to the authoritative DNS server.

---

## Authoritative Name Server

Stores the actual DNS records.

Example:

```text
google.com

↓

142.250.xxx.xxx
```

---

# 6. DNS Resolution Process

Suppose a user opens:

```text
https://google.com
```

---

## Step-by-Step Process

```text
User

↓

Browser

↓

Recursive Resolver

↓

Root Server

↓

.com TLD Server

↓

Authoritative DNS Server

↓

IP Address Returned

↓

Browser Connects to Server
```

---

## Detailed Explanation

### Step 1

User enters:

```text
google.com
```

---

### Step 2

Browser checks its local DNS cache.

If found:

```text
Use Cached IP
```

Otherwise continue.

---

### Step 3

The query is sent to the Recursive Resolver.

---

### Step 4

Resolver asks the Root Server.

Root replies:

```text
Ask the .com server.
```

---

### Step 5

Resolver asks the `.com` TLD server.

The TLD replies:

```text
Ask Google's Authoritative Server.
```

---

### Step 6

Resolver asks Google's Authoritative DNS Server.

The server returns:

```text
142.250.xxx.xxx
```

---

### Step 7

Resolver caches the answer and returns the IP to the browser.

---

### Step 8

Browser establishes a TCP connection and loads the website.

---

# 7. Recursive and Iterative Queries

DNS uses two types of queries.

---

## Recursive Query

The client asks one DNS resolver.

The resolver performs all remaining lookups.

```text
Client

↓

Recursive Resolver

↓

Complete Answer
```

The client receives only the final IP address.

---

## Iterative Query

Each DNS server responds with the next server to contact.

```text
Resolver

↓

Root

↓

TLD

↓

Authoritative Server
```

The resolver performs multiple queries until the answer is found.

---

## Difference

| Recursive Query              | Iterative Query                       |
| ---------------------------- | ------------------------------------- |
| One request from client      | Multiple requests between DNS servers |
| Resolver performs all work   | Servers return referrals              |
| Client receives final answer | Resolver continues querying           |

---

# 8. DNS Caching

To reduce lookup time, DNS stores previously resolved records.

This is called:

```text
DNS Cache
```

---

## Cache Flow

```text
DNS Query

↓

IP Found

↓

Store in Cache

↓

Future Requests

↓

Faster Response
```

---

## Benefits

* Faster browsing
* Reduced DNS traffic
* Lower latency
* Reduced load on DNS servers

---

## TTL (Time To Live)

Every DNS record contains a TTL value.

Example:

```text
TTL = 3600 Seconds
```

After TTL expires, DNS performs a fresh lookup.

---

# 9. Common DNS Records

| Record | Purpose            |
| ------ | ------------------ |
| A      | IPv4 Address       |
| AAAA   | IPv6 Address       |
| MX     | Mail Server        |
| NS     | Name Server        |
| TXT    | Text Information   |
| CNAME  | Alias              |
| SOA    | Start of Authority |
| PTR    | Reverse Lookup     |

---

## Example

```text
example.com

↓

A

↓

192.168.1.10
```

---

# 10. DNS Ports

DNS mainly uses:

| Protocol | Port | Purpose                          |
| -------- | ---- | -------------------------------- |
| UDP      | 53   | Standard DNS Queries             |
| TCP      | 53   | Zone Transfers & Large Responses |

---

## Why UDP?

Most DNS queries are very small.

UDP provides:

* Faster communication
* Less overhead

---

## Why TCP?

TCP is used when:

* Response is too large
* DNS Zone Transfer (AXFR)
* Reliability is required

---

# 11. Cybersecurity Perspective

DNS is one of the most frequently targeted protocols during cyber attacks.

---

## Common DNS Attacks

* DNS Spoofing
* DNS Cache Poisoning
* DNS Amplification
* DNS Tunneling
* DNS Hijacking
* Malicious DNS Servers

---

## Information Gathering

Attackers perform DNS enumeration to discover:

* Subdomains
* Mail Servers
* Internal Hosts
* VPN Servers
* Development Servers

---

## DNS Tunneling

Attackers may abuse DNS queries to:

* Bypass Firewalls
* Exfiltrate Data
* Establish Command & Control (C2)

---

## Zone Transfer

Misconfigured DNS servers may expose:

* Entire DNS Zone
* Internal Infrastructure
* Hidden Hosts

---

## Important Notes

* Restrict unauthorized zone transfers.
* Monitor unusual DNS traffic.
* Validate DNS records regularly.
* Use secure DNS configurations.

---

# 12. Quick Revision Sheet

DNS

```text
Domain Name System
```

---

Purpose

```text
Domain Name

↓

IP Address
```

---

DNS Hierarchy

```text
Root

↓

TLD

↓

Authoritative Server
```

---

DNS Resolution

```text
Browser

↓

Recursive Resolver

↓

Root

↓

TLD

↓

Authoritative Server

↓

IP Address
```

---

DNS Ports

```text
UDP 53

TCP 53
```

---

Common Records

```text
A

AAAA

MX

NS

TXT

CNAME

SOA

PTR
```

---

Biggest Concept

```text
DNS translates human-readable
domain names into IP addresses
through a hierarchical distributed
database consisting of recursive
resolvers, root servers,
TLD servers, and authoritative
name servers.
```

---

*End of DNS and Its Working Notes*
