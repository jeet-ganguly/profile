# Information Gathering for Web Pentesting (Part 4)

> This part covers **Web Crawling** and **Reconnaissance Automation**, which are used to discover hidden web resources automatically.
>
> Web crawlers follow links within a website and collect useful information such as pages, directories, parameters, forms, JavaScript files, images, and API endpoints. Automating reconnaissance helps penetration testers save time and ensure more complete coverage of the target.

These notes cover:

* Web Crawling
* ReconSpider
* Installing ReconSpider
* Running ReconSpider
* ReconSpider Output
* Recon Automation
* Pentesting Perspective
* Quick Revision Sheet

---

# Table of Contents

* [26. Web Crawling](#26-web-crawling)
* [27. ReconSpider](#27-reconspider)
* [28. Installing ReconSpider](#28-installing-reconspider)
* [29. Running ReconSpider](#29-running-reconspider)
* [30. Understanding ReconSpider Output](#30-understanding-reconspider-output)
* [31. Automating Reconnaissance](#31-automating-reconnaissance)
* [32. Pentesting Perspective](#32-pentesting-perspective)
* [33. Quick Revision Sheet](#33-quick-revision-sheet)

---

# 26. Web Crawling

A **Web Crawler** automatically visits web pages by following hyperlinks and collecting information from each page.

Unlike brute-forcing, crawling discovers resources that are already publicly accessible through links.

---

## Why Web Crawling?

A crawler helps discover:

* Hidden Pages
* Directories
* JavaScript Files
* Images
* CSS Files
* Forms
* API Endpoints
* Query Parameters

---

## Crawling Workflow

```text
Target Website

      │

      ▼

Homepage

      │

      ▼

Follow Links

      │

      ▼

Discover New Pages

      │

      ▼

Collect URLs & Resources
```

---

## Example

Suppose the homepage contains:

```text
https://example.com
```

The crawler discovers:

```text
https://example.com/login

https://example.com/register

https://example.com/contact

https://example.com/blog

https://example.com/api
```

These pages become potential targets during the assessment.

---

# 27. ReconSpider

ReconSpider is a web crawling script built using the **Scrapy** framework.

It automatically crawls a website and extracts useful reconnaissance information.

---

## What Can ReconSpider Discover?

ReconSpider can collect:

* URLs
* Directories
* Images
* JavaScript Files
* Forms
* External Links
* Internal Links
* Email Addresses (if publicly available)

---

## Why Use ReconSpider?

Manual browsing often misses hidden pages.

ReconSpider automatically explores the website and creates a structured list of discovered resources.

---

# 28. Installing ReconSpider

ReconSpider requires **Scrapy**.

---

## Install Scrapy

```bash
pip3 install scrapy
```

---

## Download ReconSpider

```bash
wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip
```

---

## Extract Files

```bash
unzip ReconSpider.zip
```

---

## Verify Installation

After extraction, the project directory contains:

```text
ReconSpider.py

requirements

configuration files

output directory
```

---

# 29. Running ReconSpider

Basic Syntax:

```bash
python3 ReconSpider.py <URL>
```

Example:

```bash
python3 ReconSpider.py https://example.com
```

The crawler starts from the supplied URL and recursively follows links throughout the website.

---

## Crawling Process

```text
Start URL

      │

      ▼

Download Page

      │

      ▼

Extract Links

      │

      ▼

Visit New Links

      │

      ▼

Repeat Until No New Links Exist
```

---

# 30. Understanding ReconSpider Output

After crawling, ReconSpider produces a collection of discovered resources.

Typical findings include:

* Internal URLs
* External URLs
* Images
* JavaScript Files
* CSS Files
* Forms
* Parameters

---

## Example Output

```text
/login

/register

/admin

/contact

/api/v1

/assets/app.js

/images/logo.png
```

Each discovered resource should be examined during later testing phases.

---

## Why is This Valuable?

Discovered URLs may reveal:

* Hidden Login Pages
* API Endpoints
* Administrative Panels
* Backup Directories
* Forgotten Resources

---

# 31. Automating Reconnaissance

Modern penetration testing often combines multiple reconnaissance tools.

Instead of performing every task manually, automation speeds up information gathering and improves coverage.

---

## Typical Automated Recon Workflow

```text
Target Domain

      │

      ▼

WHOIS

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

Web Crawling

      │

      ▼

Vulnerability Assessment
```

---

## Benefits of Automation

Automation helps:

* Reduce Manual Work
* Discover More Assets
* Save Time
* Produce Repeatable Results
* Improve Coverage

---

## Important Note

Automation **does not replace manual testing**.

Automated tools collect information quickly, but manual verification is still required to:

* Validate Findings
* Remove False Positives
* Identify Business Logic Issues
* Discover Complex Vulnerabilities

---

## HTB Recommendation

For a complete automated reconnaissance workflow, study the **Hack The Box Academy – Web Information Gathering** module.

It combines:

* WHOIS
* DNS Enumeration
* Subdomain Enumeration
* Virtual Host Enumeration
* Fingerprinting
* Web Crawling

into a complete reconnaissance methodology.

---

# 32. Pentesting Perspective

Reconnaissance determines the overall quality of a penetration test.

Missing an important asset may result in missing a critical vulnerability.

---

## Always Enumerate Before Exploitation

Never begin vulnerability testing before completing reconnaissance.

Good enumeration often reveals:

* Hidden Applications
* Test Environments
* APIs
* Administrative Interfaces
* Legacy Systems

---

## Combine Multiple Techniques

Do not rely on a single tool.

Combine:

* WHOIS
* DIG
* DNSENUM
* Gobuster
* cURL
* WAFW00F
* Nikto
* ReconSpider

Each tool reveals different information.

---

## Validate Automated Results

Automation may produce:

* Duplicate URLs
* Dead Links
* Redirects
* False Positives

Always verify results manually before proceeding.

---

## Continue Recon Throughout Testing

Reconnaissance is not a one-time activity.

As new hosts, applications, or APIs are discovered, perform additional enumeration.

---

## Important Notes

* Good reconnaissance often leads to successful exploitation.
* Small hidden applications frequently contain the most critical vulnerabilities.
* Automation improves efficiency but manual analysis remains essential.
* Record all discovered assets for later testing.

---

# 33. Quick Revision Sheet

Install Scrapy

```bash
pip3 install scrapy
```

---

Download ReconSpider

```bash
wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip
```

---

Extract Files

```bash
unzip ReconSpider.zip
```

---

Run ReconSpider

```bash
python3 ReconSpider.py https://example.com
```

---

Recon Workflow

```text
WHOIS

↓

DNS Enumeration

↓

Subdomain Enumeration

↓

Virtual Host Enumeration

↓

Technology Fingerprinting

↓

Web Crawling

↓

Vulnerability Assessment
```

---

Most Useful Recon Tools

| Tool          | Purpose                         |
| ------------- | ------------------------------- |
| `whois`       | Domain Registration Information |
| `dig`         | DNS Enumeration                 |
| `dnsenum`     | Subdomain Enumeration           |
| `gobuster`    | Virtual Host Enumeration        |
| `curl`        | HTTP Fingerprinting             |
| `wafw00f`     | WAF Detection                   |
| `nikto`       | Software Identification         |
| `ReconSpider` | Automated Web Crawling          |

---

Biggest Concept

```text
Effective web penetration testing
begins with thorough reconnaissance.

Combining DNS enumeration,
subdomain discovery,
virtual host enumeration,
fingerprinting,
and web crawling provides
a comprehensive understanding
of the target before
vulnerability assessment begins.
```

---

*End of Information Gathering for Web Pentesting (Part 4)*
