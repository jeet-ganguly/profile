# Information Gathering for Web Pentesting (Part 2)

> This part continues the Information Gathering phase by covering techniques used to discover hidden subdomains and archived URLs.
>
> During web penetration testing, many applications expose additional attack surfaces through forgotten subdomains, development environments, staging servers, and archived web pages. Proper subdomain enumeration significantly increases the chances of finding vulnerable assets.

These notes cover:

* Subdomain Enumeration
* Active vs Passive Enumeration
* DNSENUM
* Recursive Enumeration
* Wayback Machine Enumeration
* Common Wordlists
* Pentesting Perspective
* Quick Revision Sheet

---

# Table of Contents

* [12. Subdomain Enumeration](#12-subdomain-enumeration)
* [13. Active vs Passive Enumeration](#13-active-vs-passive-enumeration)
* [14. DNSENUM](#14-dnsenum)
* [15. Recursive Enumeration](#15-recursive-enumeration)
* [16. Wayback Machine Enumeration](#16-wayback-machine-enumeration)
* [17. Common Wordlists](#17-common-wordlists)
* [18. Pentesting Perspective](#18-pentesting-perspective)
* [19. Quick Revision Sheet](#19-quick-revision-sheet)

---

# 12. Subdomain Enumeration

A subdomain is a child domain of the main domain.

Example:

```text
example.com

├── www.example.com

├── mail.example.com

├── api.example.com

├── admin.example.com

├── dev.example.com

└── test.example.com
```

Many organizations host different services on different subdomains.

---

## Why Enumerate Subdomains?

Hidden subdomains often expose:

* Development Servers
* Testing Environments
* APIs
* Admin Panels
* VPN Portals
* Internal Applications

Sometimes these hosts have weaker security than the primary website.

---

## Enumeration Workflow

```text
Target Domain

      │

      ▼

Wordlist

      │

      ▼

DNS Brute Force

      │

      ▼

Valid Subdomains

      │

      ▼

Attack Surface Expansion
```

---

# 13. Active vs Passive Enumeration

Subdomain enumeration can be divided into two categories.

---

## Active Enumeration

The tester directly interacts with the target's infrastructure.

Examples:

* DNS Brute Force
* DNS Queries
* Zone Transfer Attempts

Advantages:

* Finds new assets
* More accurate

Disadvantages:

* Generates network traffic
* May be detected

---

## Passive Enumeration

Information is collected from publicly available sources without directly interacting with the target.

Examples:

* Search Engines
* Certificate Transparency Logs
* Wayback Machine
* Public Datasets

Advantages:

* Stealthier
* Less likely to trigger alerts

Disadvantages:

* May miss recently created subdomains

---

# 14. DNSENUM

`dnsenum` is an automated DNS enumeration tool.

It can:

* Discover Subdomains
* Query DNS Records
* Attempt Zone Transfers
* Perform Reverse Lookups
* Brute Force DNS

---

## Basic Enumeration

Syntax:

```bash
dnsenum --enum <domain_name> -f <wordlist>
```

Example:

```bash
dnsenum --enum example.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

This command:

* Enumerates the target domain
* Uses the supplied wordlist
* Attempts to discover valid subdomains

---

## Recursive Enumeration

Syntax:

```bash
dnsenum --enum <domain_name> -f <wordlist> -r
```

Example:

```bash
dnsenum --enum example.com \
-f /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
-r
```

The `-r` option enables **recursive enumeration**.

---

## How Recursive Enumeration Works

Suppose the first scan discovers:

```text
dev.example.com
```

With recursive mode enabled:

```text
dev.example.com

↓

admin.dev.example.com

↓

api.dev.example.com

↓

test.dev.example.com
```

Each newly discovered subdomain becomes another target for enumeration.

---

## Without Recursive Mode

```text
example.com

↓

mail.example.com

↓

api.example.com

↓

dev.example.com
```

Enumeration stops here.

---

## With Recursive Mode

```text
example.com

↓

dev.example.com

↓

api.dev.example.com

↓

beta.api.dev.example.com
```

The attack surface becomes much larger.

---

# 15. Wayback Machine Enumeration

Sometimes deleted pages and subdomains remain archived on the Internet.

The **Wayback Machine** can reveal:

* Old URLs
* Hidden Directories
* Deprecated APIs
* Forgotten Subdomains

---

## Query Wayback Machine

```bash
curl -G "https://web.archive.org/cdx/search/cdx" \
--data-urlencode "url=*.example.com/*" \
--data-urlencode "collapse=urlkey" \
--data-urlencode "output=text" \
--data-urlencode "fl=original"
```

---

## Command Breakdown

| Option            | Purpose                       |
| ----------------- | ----------------------------- |
| `-G`              | Send GET request              |
| `url=*.`          | Search all subdomains         |
| `collapse=urlkey` | Remove duplicate URLs         |
| `output=text`     | Plain text output             |
| `fl=original`     | Return original archived URLs |

---

## Example Output

```text
https://old.example.com/login

https://admin.example.com

https://dev.example.com

https://api.example.com/v1
```

Even if these URLs no longer exist, they provide valuable intelligence.

---

## Why is Wayback Useful?

Archived pages may reveal:

* Old Admin Panels
* Hidden Endpoints
* Deprecated APIs
* Backup Files
* Legacy Applications

---

# 16. Common Wordlists

Subdomain brute-forcing relies on good wordlists.

Common locations:

```text
/usr/share/seclists/Discovery/DNS/
```

Popular wordlists include:

```text
subdomains-top1million-5000.txt

subdomains-top1million-110000.txt
```

Smaller wordlists:

* Faster
* Fewer Requests

Larger wordlists:

* Better Coverage
* Slower Enumeration

---

## Choosing a Wordlist

| Wordlist Size | Best For             |
| ------------- | -------------------- |
| Small         | Initial Recon        |
| Medium        | Standard Enumeration |
| Large         | Thorough Assessments |

---

# 17. Pentesting Perspective

Subdomain enumeration is one of the most valuable reconnaissance techniques.

---

## Interesting Subdomains

Look for:

```text
admin

dev

test

staging

vpn

mail

portal

backup

old

beta

internal
```

These often expose valuable attack surfaces.

---

## Combine Multiple Techniques

Do not rely on only one method.

Combine:

* DNS Queries
* DNSENUM
* Wayback Machine
* Certificate Transparency Logs
* Search Engines

The more techniques used, the more complete the reconnaissance.

---

## Recursive Enumeration

Always consider recursive enumeration because many organizations use nested subdomains.

Example:

```text
api.dev.example.com
```

instead of simply:

```text
dev.example.com
```

---

## Information Leakage

Archived URLs may reveal:

* API Keys
* Old Login Pages
* Configuration Files
* Backup Directories
* Hidden Applications

---

## Important Notes

* Begin with passive enumeration before active scanning.
* Use smaller wordlists initially to reduce unnecessary requests.
* Increase wordlist size only if additional coverage is required.
* Archived URLs should always be verified because some may no longer exist.

---

# 18. Quick Revision Sheet

Basic Enumeration

```bash
dnsenum --enum example.com -f <wordlist>
```

---

Recursive Enumeration

```bash
dnsenum --enum example.com -f <wordlist> -r
```

---

Wayback Machine Enumeration

```bash
curl -G "https://web.archive.org/cdx/search/cdx" \
--data-urlencode "url=*.example.com/*" \
--data-urlencode "collapse=urlkey" \
--data-urlencode "output=text" \
--data-urlencode "fl=original"
```

---

Useful Wordlists

```text
/usr/share/seclists/Discovery/DNS/

subdomains-top1million-5000.txt

subdomains-top1million-110000.txt
```

---

Biggest Concept

```text
Subdomain enumeration expands the
attack surface by discovering hidden
hosts, development environments,
APIs, and archived web assets that
may contain exploitable vulnerabilities.
```

---

*End of Information Gathering for Web Pentesting (Part 2)*
