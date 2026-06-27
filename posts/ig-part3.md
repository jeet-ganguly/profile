# Information Gathering for Web Pentesting (Part 3)

> This part covers techniques used to identify virtual hosts, fingerprint web technologies, detect Web Application Firewalls (WAFs), and identify web server software.
>
> Before searching for vulnerabilities, a penetration tester should understand what technologies the target is using. This helps select the right attack techniques and reduces unnecessary testing.

These notes cover:

* Virtual Host Enumeration
* Gobuster VHOST Mode
* HTTP Fingerprinting
* cURL Header Analysis
* WAF Detection
* Nikto Software Identification
* Pentesting Perspective
* Quick Revision Sheet

---

# Table of Contents

* [19. Virtual Host Enumeration](#19-virtual-host-enumeration)
* [20. Gobuster VHOST Mode](#20-gobuster-vhost-mode)
* [21. HTTP Fingerprinting](#21-http-fingerprinting)
* [22. WAF Detection](#22-waf-detection)
* [23. Nikto Software Identification](#23-nikto-software-identification)
* [24. Pentesting Perspective](#24-pentesting-perspective)
* [25. Quick Revision Sheet](#25-quick-revision-sheet)

---

# 19. Virtual Host Enumeration

Many web servers host multiple websites on the same IP address.

These websites are called **Virtual Hosts (VHosts)**.

Example:

```text
192.168.1.10
      │
      ├── www.example.com
      ├── admin.example.com
      ├── api.example.com
      └── dev.example.com
```

Although all websites share the same IP address, each serves different content based on the **Host** header.

---

## Why Enumerate Virtual Hosts?

Sometimes hidden applications exist only as virtual hosts.

Examples:

```text
admin.example.com

test.example.com

dev.example.com

internal.example.com
```

These hosts may not appear in public DNS records.

---

## How Does It Work?

When a browser accesses:

```text
https://admin.example.com
```

it sends:

```http
Host: admin.example.com
```

The web server reads the **Host** header and serves the corresponding website.

---

# 20. Gobuster VHOST Mode

Gobuster can brute-force virtual host names using a wordlist.

Syntax:

```bash
gobuster vhost -u http://<target_ip> -w <wordlist> --append-domain
```

Example:

```bash
gobuster vhost \
-u http://192.168.1.20 \
-w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt \
--append-domain
```

---

## Command Explanation

| Option            | Description                          |
| ----------------- | ------------------------------------ |
| `vhost`           | Virtual Host Enumeration Mode        |
| `-u`              | Target URL or IP Address             |
| `-w`              | Wordlist                             |
| `--append-domain` | Appends the base domain to each word |

---

## Useful Options

Increase Threads

```bash
-t 50
```

Ignore SSL Errors

```bash
-k
```

Save Output

```bash
-o result.txt
```

---

## Example

```bash
gobuster vhost \
-u https://example.com \
-w wordlist.txt \
--append-domain \
-t 50 \
-o results.txt
```

---

## Workflow

```text
Wordlist

     │

     ▼

Host Header

     │

     ▼

Web Server

     │

     ▼

Valid Virtual Host
```

---

# 21. HTTP Fingerprinting

HTTP Fingerprinting is the process of identifying the technologies used by a web server.

Examples:

* Web Server
* Framework
* CMS
* Programming Language
* Security Headers

---

## Using cURL

The easiest way is using:

```bash
curl -I <URL>
```

Example:

```bash
curl -I https://example.com
```

---

## Example Response

```http
HTTP/1.1 200 OK

Server: nginx

Content-Type: text/html

Content-Length: 2510

X-Powered-By: PHP/8.2
```

---

## Information Obtained

Possible information includes:

* Web Server

```text
Apache

Nginx

IIS
```

---

Programming Language

```text
PHP

ASP.NET

Java

Python
```

---

Content Type

```text
text/html

application/json
```

---

Security Headers

```text
Content-Security-Policy

X-Frame-Options

Strict-Transport-Security

X-Content-Type-Options
```

---

## Why Fingerprinting?

Knowing the technology stack helps choose appropriate vulnerability tests.

Example:

```text
Apache

↓

Apache Vulnerabilities

↓

Specific Exploitation
```

---

# 22. WAF Detection

Many organizations protect web applications using a **Web Application Firewall (WAF).**

A WAF filters malicious HTTP requests before they reach the web server.

---

## Detecting WAF

Tool:

```text
wafw00f
```

Installation:

```bash
pip3 install git+https://github.com/EnableSecurity/wafw00f
```

---

## Basic Scan

```bash
wafw00f https://example.com
```

---

## Example Output

```text
Checking https://example.com

The site is behind Cloudflare WAF
```

---

## Common WAFs

| WAF           | Vendor      |
| ------------- | ----------- |
| Cloudflare    | Cloudflare  |
| AWS WAF       | Amazon      |
| Imperva       | Imperva     |
| F5 BIG-IP ASM | F5          |
| Akamai Kona   | Akamai      |
| ModSecurity   | Open Source |

---

## Why Detect a WAF?

A WAF may:

* Block Payloads
* Filter SQL Injection
* Detect XSS Attempts
* Block Directory Enumeration
* Rate Limit Requests

Knowing a WAF exists helps explain why some requests are blocked.

---

# 23. Nikto Software Identification

Nikto is a web server scanner.

Besides vulnerability checks, it can identify server software.

---

## Software Identification Only

Syntax:

```bash
nikto -h <target> -Tuning b
```

Example:

```bash
nikto -h https://example.com -Tuning b
```

---

## What Does `-Tuning b` Do?

Runs only the **Software Identification** modules.

This reduces unnecessary scanning and focuses on identifying technologies.

---

## Information Obtained

Nikto may identify:

* Web Server
* Server Version
* Installed Software
* Common Technologies
* Security Headers

---

## Example Output

```text
Server: nginx

PHP: 8.2

X-Powered-By: PHP
```

---

# 24. Pentesting Perspective

Technology fingerprinting is one of the most important reconnaissance activities.

---

## Identify the Web Server

Determine whether the application uses:

```text
Apache

Nginx

Microsoft IIS

LiteSpeed
```

Each server has different configurations and potential weaknesses.

---

## Identify the Programming Language

Common examples:

```text
PHP

ASP.NET

Java

Python

Node.js
```

This helps narrow down possible vulnerabilities.

---

## Look for Missing Security Headers

Using:

```bash
curl -I
```

verify whether important security headers are present.

Examples:

* Content-Security-Policy
* Strict-Transport-Security
* X-Frame-Options
* X-Content-Type-Options

Missing headers may indicate security hardening opportunities.

---

## Detect WAF Before Testing

Knowing that a WAF exists helps explain:

* Blocked Requests
* CAPTCHA Challenges
* HTTP 403 Responses
* Rate Limiting

---

## Enumerate Hidden Virtual Hosts

Development or staging sites often exist as hidden virtual hosts.

Examples:

```text
dev.example.com

admin.example.com

beta.example.com

internal.example.com
```

These may expose vulnerable applications.

---

## Important Notes

* Fingerprinting should always be performed before vulnerability testing.
* Combine multiple tools because each reveals different information.
* Do not rely on a single HTTP header for technology identification; servers may intentionally hide or modify version information.
* Hidden virtual hosts often provide a larger attack surface than the main website.

---

# 25. Quick Revision Sheet

Virtual Host Enumeration

```bash
gobuster vhost -u http://<IP> -w <wordlist> --append-domain
```

---

Increase Threads

```bash
-t 50
```

---

Ignore SSL Verification

```bash
-k
```

---

Save Output

```bash
-o results.txt
```

---

HTTP Headers

```bash
curl -I https://example.com
```

---

Detect WAF

```bash
wafw00f https://example.com
```

---

Software Identification

```bash
nikto -h https://example.com -Tuning b
```

---

Most Useful Tools

| Tool       | Purpose                         |
| ---------- | ------------------------------- |
| `gobuster` | Virtual Host Enumeration        |
| `curl`     | HTTP Header Analysis            |
| `wafw00f`  | Detect Web Application Firewall |
| `nikto`    | Software Identification         |

---

Biggest Concept

```text
Technology fingerprinting and
virtual host enumeration help
identify hidden applications,
web technologies, and security
controls before vulnerability testing,
allowing more targeted and efficient
web penetration testing.
```

---

*End of Information Gathering for Web Pentesting (Part 3)*
