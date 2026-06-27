# Web Requests with cURL

> **cURL (Client URL)** is a command-line tool used to transfer data between a client and a server using various protocols such as **HTTP**, **HTTPS**, **FTP**, **SMTP**, and more.
>
> In web penetration testing, cURL is one of the most important tools for manually interacting with web servers, testing APIs, sending custom HTTP requests, and automating repetitive tasks.

These notes cover:

* Introduction to cURL
* Basic GET Request
* HTTPS and SSL Verification
* Verbose Mode
* HEAD Request
* Custom User-Agent
* Basic Authentication
* Custom HTTP Headers
* Custom HTTP Methods
* Common HTTP Methods
* Pentesting Perspective
* Quick Revision Sheet

---

# Table of Contents

* [1. Introduction to cURL](#1-introduction-to-curl)
* [2. Basic GET Request](#2-basic-get-request)
* [3. HTTPS and SSL Verification](#3-https-and-ssl-verification)
* [4. Verbose Mode](#4-verbose-mode)
* [5. HEAD Request](#5-head-request)
* [6. Custom User-Agent](#6-custom-user-agent)
* [7. Basic Authentication](#7-basic-authentication)
* [8. Custom HTTP Headers](#8-custom-http-headers)
* [9. Custom HTTP Methods](#9-custom-http-methods)
* [10. Common HTTP Methods](#10-common-http-methods)
* [11. Pentesting Perspective](#11-pentesting-perspective)
* [12. Quick Revision Sheet](#12-quick-revision-sheet)

---

# 1. Introduction to cURL

**cURL (Client URL)** is a command-line utility used to send HTTP requests and receive responses directly from a web server.

Unlike a web browser, cURL allows complete control over the request.

It is commonly used for:

* Web Penetration Testing
* API Testing
* Authentication Testing
* Header Manipulation
* Automation
* Debugging Web Applications

---

## Basic Syntax

```bash
curl [options] <URL>
```

Example:

```bash
curl https://example.com
```

---

## Default HTTP Method

If no method is specified, cURL sends a:

```text
GET Request
```

---

# 2. Basic GET Request

A GET request retrieves data from a web server.

Syntax:

```bash
curl <domain_name>
```

Example:

```bash
curl https://example.com
```

Output:

```text
<HTML Response>

or

JSON Response
```

---

## When to Use?

* View webpage source
* Test APIs
* Download page content
* Verify server response

---

# 3. HTTPS and SSL Verification

By default, cURL verifies the server's SSL/TLS certificate before establishing a secure HTTPS connection.

If the certificate is:

* Invalid
* Expired
* Self-Signed

cURL stops the connection to protect against **Man-in-the-Middle (MITM)** attacks.

---

## Ignore SSL Certificate Verification

Syntax:

```bash
curl -k <domain_name>
```

Example:

```bash
curl -k https://example.local
```

The `-k` option tells cURL to ignore SSL certificate validation.

---

## When is `-k` Useful?

* Local development
* Lab environments
* HTB Machines
* CTF Challenges
* Testing Self-Signed Certificates

---

## Important Note

Avoid using:

```bash
curl -k
```

on production systems because SSL verification provides important security protection.

---

# 4. Verbose Mode

Verbose mode displays both the HTTP request and HTTP response.

Syntax:

```bash
curl -v <domain_name>
```

Example:

```bash
curl -v https://example.com
```

---

## Information Displayed

* DNS Resolution
* TCP Connection
* TLS Handshake
* Request Headers
* Response Headers
* HTTP Status Code

---

## Why Use Verbose Mode?

Useful for:

* Debugging HTTP Requests
* Viewing Redirects
* Inspecting Headers
* Troubleshooting SSL Issues

---

# 5. HEAD Request

Sometimes we only need the response headers without downloading the entire webpage.

Use the `-I` option.

Syntax:

```bash
curl -I <domain_name>
```

Example:

```bash
curl -I https://example.com
```

---

## Example Output

```text
HTTP/1.1 200 OK

Server: nginx

Content-Type: text/html

Content-Length: 3456
```

---

## Why Use HEAD Requests?

Useful for checking:

* Server Information
* Content Type
* Content Length
* Cache Headers
* Security Headers

without downloading the webpage.

---

# 6. Custom User-Agent

A **User-Agent** identifies the client making the HTTP request.

By default, cURL sends its own User-Agent.

You can replace it using the `-A` option.

Syntax:

```bash
curl -A "<User-Agent>" <URL>
```

Example:

```bash
curl -A "Mozilla/5.0" https://example.com
```

---

## Why Change User-Agent?

Some websites:

* Block cURL Requests
* Serve Different Content
* Detect Bots

Changing the User-Agent helps simulate requests from a browser.

---

# 7. Basic Authentication

Some web applications require a username and password.

Use the `-u` option.

Syntax:

```bash
curl -u <username>:<password> <URL>
```

Example:

```bash
curl -u admin:password https://example.com/login
```

cURL automatically generates the required HTTP **Authorization** header.

---

# 8. Custom HTTP Headers

Many web applications and APIs require custom HTTP headers.

Use the `-H` option.

Syntax:

```bash
curl -H "Header: Value" <URL>
```

Example:

```bash
curl -H "Authorization: Bearer TOKEN" https://example.com/api
```

---

## Common Headers

| Header        | Purpose                |
| ------------- | ---------------------- |
| Authorization | Authentication         |
| Cookie        | Session Management     |
| Content-Type  | Data Format            |
| Accept        | Expected Response Type |
| Referer       | Previous Page          |
| Origin        | Cross-Origin Requests  |
| User-Agent    | Client Information     |

---

## Multiple Headers

```bash
curl \
-H "Authorization: Bearer TOKEN" \
-H "Content-Type: application/json" \
https://example.com/api
```

---

# 9. Custom HTTP Methods

By default, cURL sends a GET request.

To use another HTTP method, use the `-X` option.

Syntax:

```bash
curl -X <METHOD> <URL>
```

Example:

```bash
curl -X POST https://example.com/login
```

---

## Common Examples

GET

```bash
curl -X GET https://example.com
```

POST

```bash
curl -X POST https://example.com
```

PUT

```bash
curl -X PUT https://example.com
```

DELETE

```bash
curl -X DELETE https://example.com
```

PATCH

```bash
curl -X PATCH https://example.com
```

---

# 10. Common HTTP Methods

| Method  | Purpose                        |
| ------- | ------------------------------ |
| GET     | Retrieve Data                  |
| POST    | Submit Data                    |
| PUT     | Update Existing Resource       |
| PATCH   | Partially Update Resource      |
| DELETE  | Delete Resource                |
| HEAD    | Retrieve Response Headers Only |
| OPTIONS | Display Supported Methods      |

---

# 11. Pentesting Perspective

cURL is one of the most frequently used tools during web penetration testing.

---

## Common Uses

* Test REST APIs
* Inspect HTTP Headers
* Test Authentication
* Modify Request Methods
* Change User-Agent
* Test Authorization
* Inspect Security Headers
* Validate HTTPS Configuration

---

## Information Security Testers Commonly Check

Using:

```bash
curl -I
```

look for security headers such as:

* Strict-Transport-Security
* X-Frame-Options
* X-Content-Type-Options
* Content-Security-Policy

---

## API Testing

Use:

```bash
curl -H
```

to send:

* Authorization Tokens
* Cookies
* API Keys
* Custom Headers

---

## Authentication Testing

Use:

```bash
curl -u
```

to test HTTP Basic Authentication.

---

## HTTPS Testing

Use:

```bash
curl -v
```

to inspect:

* TLS Version
* Certificate Details
* Redirects
* Response Headers

---

## Important Notes

* Avoid using `-k` on production environments.
* Always verify server certificates whenever possible.
* Never expose API keys or passwords directly in shared command history.
* Review HTTP response headers for missing security controls.

---

# 12. Quick Revision Sheet

Basic Request:

```bash
curl https://example.com
```

---

Ignore SSL Verification:

```bash
curl -k https://example.com
```

---

Verbose Mode:

```bash
curl -v https://example.com
```

---

HEAD Request:

```bash
curl -I https://example.com
```

---

Custom User-Agent:

```bash
curl -A "Mozilla/5.0" https://example.com
```

---

Basic Authentication:

```bash
curl -u admin:password https://example.com
```

---

Custom Header:

```bash
curl -H "Authorization: Bearer TOKEN" https://example.com
```

---

Custom HTTP Method:

```bash
curl -X POST https://example.com
```

---

Most Useful Flags

| Flag | Purpose                 |
| ---- | ----------------------- |
| `-k` | Ignore SSL Verification |
| `-v` | Verbose Output          |
| `-I` | Send HEAD Request       |
| `-A` | Set User-Agent          |
| `-u` | Basic Authentication    |
| `-H` | Custom Header           |
| `-X` | Specify HTTP Method     |

---

Biggest Concept:

```text
cURL allows complete control
over HTTP requests, making it
one of the most important tools
for web penetration testing,
API testing, and web debugging.
```

---

*End of Web Requests with cURL Notes*
