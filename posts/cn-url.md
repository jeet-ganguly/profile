# Computer Networks — URL Format & HTTP Headers

> Every HTTP request begins with a **URL (Uniform Resource Locator)**, which tells the browser where to send the request. Along with the URL, the browser also sends **HTTP Headers** that provide additional information such as browser type, authentication, cookies, accepted content types, and more.
>
> Understanding URLs and HTTP headers is essential for **Web Development**, **API Testing**, **Bug Bounty Hunting**, and **Web Penetration Testing**.

These notes cover:

- What is a URL?
- URL Structure
- URL Encoding
- Absolute vs Relative URL
- Query Parameters
- URL Fragments
- HTTP Headers
- Request Headers
- Response Headers
- Common Security Headers
- Cybersecurity Perspective
- Quick Revision Sheet

---

# Table of Contents

- [1. What is a URL?](#1-what-is-a-url)
- [2. URL Structure](#2-url-structure)
- [3. URL Components](#3-url-components)
- [4. Absolute vs Relative URL](#4-absolute-vs-relative-url)
- [5. Query Parameters](#5-query-parameters)
- [6. URL Encoding](#6-url-encoding)
- [7. URL Fragments](#7-url-fragments)
- [8. What are HTTP Headers?](#8-what-are-http-headers)
- [9. HTTP Request Headers](#9-http-request-headers)
- [10. HTTP Response Headers](#10-http-response-headers)
- [11. Common Security Headers](#11-common-security-headers)
- [12. Cybersecurity Perspective](#12-cybersecurity-perspective)
- [13. Quick Revision Sheet](#13-quick-revision-sheet)

---

# 1. What is a URL?

URL stands for:

```text
Uniform Resource Locator
```

A URL is the complete address used to locate a resource on the Internet.

Resources include:

- Web Pages
- Images
- Videos
- APIs
- Files
- Documents

---

## Example URL

```text
https://username:password@www.example.com:443/products/laptop?id=15&color=black#reviews
```

This single URL tells the browser:

- Which protocol to use
- Which server to connect to
- Which resource to request
- Which parameters to send

---

# 2. URL Structure

A complete URL consists of several components.

```text
https://www.example.com:443/products/laptop?id=15&color=black#reviews
│        │               │        │                     │
│        │               │        │                     └── Fragment
│        │               │        └──────────────────────── Query String
│        │               └───────────────────────────────── Path
│        └──────────────────────────────────────────────── Host
└───────────────────────────────────────────────────────── Scheme
```

---

# 3. URL Components

## Scheme (Protocol)

The scheme tells the browser which protocol should be used.

Examples:

```text
http

https

ftp

file
```

Example:

```text
https://example.com
```

uses HTTPS.

---

## Host (Domain)

The host identifies the destination server.

Example:

```text
www.example.com
```

The browser resolves this domain into an IP address using DNS.

---

## Port

The port identifies the network service running on the server.

Example:

```text
https://example.com:8443
```

Common ports:

| Protocol | Port |
|----------|------|
| HTTP | 80 |
| HTTPS | 443 |
| FTP | 21 |

If no port is specified:

- HTTP → 80
- HTTPS → 443

---

## Path

The path specifies the requested resource.

Example:

```text
/products/laptop
```

Possible paths:

```text
/index.html

/login

/admin

/api/users

/images/logo.png
```

---

## Query String

A query string sends data to the server.

Example:

```text
?id=15&color=black
```

Contains:

```text
Parameter = Value
```

---

## Multiple Parameters

```text
/search?name=jeet&country=india&page=2
```

Parameters:

| Parameter | Value |
|-----------|-------|
| name | jeet |
| country | india |
| page | 2 |

---

## Fragment

The fragment begins with:

```text
#
```

Example:

```text
#reviews
```

Fragments are processed by the browser and **are not sent to the web server**.

---

# 4. Absolute vs Relative URL

## Absolute URL

Contains the complete address.

Example:

```text
https://example.com/images/logo.png
```

Includes:

- Protocol
- Domain
- Path

---

## Relative URL

Contains only the path.

Example:

```text
/images/logo.png
```

The browser automatically appends the current domain.

---

## Comparison

| Absolute URL | Relative URL |
|--------------|--------------|
| Complete Address | Path Only |
| Includes Domain | No Domain |
| Can Access Any Website | Only Current Website |

---

# 5. Query Parameters

Query parameters pass additional information to the server.

Example:

```text
https://example.com/search?q=laptop&page=2
```

Server receives:

```text
q = laptop

page = 2
```

---

## Common Uses

- Search
- Filters
- Pagination
- Sorting
- API Requests

---

## Multiple Parameters

```text
?username=admin&role=user&id=25
```

Each parameter is separated using:

```text
&
```

---

# 6. URL Encoding

URLs cannot contain every character directly.

Special characters are encoded using **Percent Encoding**.

---

## Examples

| Character | Encoded |
|-----------|----------|
| Space | `%20` |
| @ | `%40` |
| / | `%2F` |
| : | `%3A` |
| ? | `%3F` |
| & | `%26` |
| = | `%3D` |
| + | `%2B` |
| # | `%23` |

---

## Example

Original:

```text
John Doe
```

Encoded:

```text
John%20Doe
```

---

## Why URL Encoding?

Allows special characters to be transmitted safely over HTTP.

---

# 7. URL Fragments

A fragment points to a specific section of a webpage.

Example:

```text
https://example.com/docs#installation
```

Browser opens:

```text
Installation Section
```

---

## Important Note

Fragments:

- Are processed only by the browser.
- Are **never** sent to the web server.

---

# 8. What are HTTP Headers?

HTTP Headers are additional pieces of information exchanged between the client and server.

Headers describe:

- Browser Information
- Authentication
- Cookies
- Content Type
- Compression
- Security Policies

---

## HTTP Communication

```text
Browser

↓

Request Headers

↓

Web Server

↓

Response Headers

↓

Browser
```

---

# 9. HTTP Request Headers

Request headers are sent from the client to the server.

Example:

```http
GET / HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
Authorization: Bearer TOKEN
Cookie: session=abc123
```

---

## Common Request Headers

### Host

Specifies the destination domain.

Example:

```http
Host: example.com
```

---

### User-Agent

Identifies the client software.

Example:

```http
User-Agent: Mozilla/5.0
```

---

### Accept

Specifies the response format accepted by the client.

Example:

```http
Accept: application/json
```

---

### Authorization

Used for authentication.

Example:

```http
Authorization: Bearer TOKEN
```

---

### Cookie

Sends session information.

Example:

```http
Cookie: session=abc123
```

---

### Referer

Indicates the previous webpage.

Example:

```http
Referer: https://example.com/login
```

---

### Origin

Indicates where the request originated.

Mostly used for:

- CORS
- API Requests

Example:

```http
Origin: https://example.com
```

---

### Content-Type

Specifies the format of the request body.

Examples:

```http
Content-Type: application/json

Content-Type: multipart/form-data

Content-Type: application/xml
```

---

# 10. HTTP Response Headers

Response headers are returned by the web server.

Example:

```http
HTTP/1.1 200 OK
Server: nginx
Content-Type: text/html
Content-Length: 5230
Set-Cookie: session=abc123
```

---

## Common Response Headers

### Server

Identifies the web server.

Example:

```http
Server: nginx
```

---

### Content-Type

Indicates the type of returned content.

Example:

```http
Content-Type: application/json
```

---

### Content-Length

Size of the response body.

Example:

```http
Content-Length: 2048
```

---

### Set-Cookie

Creates a cookie on the client.

Example:

```http
Set-Cookie: session=abc123
```

---

### Location

Used during redirects.

Example:

```http
Location: /login
```

---

# 11. Common Security Headers

Security headers help protect web applications against common attacks.

| Header | Purpose |
|----------|----------|
| Content-Security-Policy | Prevent XSS |
| Strict-Transport-Security | Force HTTPS |
| X-Frame-Options | Prevent Clickjacking |
| X-Content-Type-Options | Prevent MIME Sniffing |
| Referrer-Policy | Control Referer Information |
| Permissions-Policy | Restrict Browser Features |

---

## Why Security Headers Matter?

Proper security headers reduce the risk of:

- Cross-Site Scripting (XSS)
- Clickjacking
- MIME Sniffing
- Information Disclosure
- Mixed Content Attacks

---

# 12. Cybersecurity Perspective

URLs and HTTP headers are among the first things analyzed during web penetration testing.

---

## URL Analysis

Check for:

- Hidden Parameters
- IDOR Opportunities
- Sensitive Endpoints
- Debug Pages
- Admin Interfaces

---

## Parameter Testing

Common targets include:

```text
id

user

page

file

redirect

url

token
```

These parameters are frequently tested for:

- SQL Injection
- XSS
- LFI
- SSRF
- IDOR
- Open Redirect

---

## Header Analysis

Inspect request and response headers for:

- Authentication Tokens
- Cookies
- Server Information
- Missing Security Headers
- CORS Configuration

---

## Common Security Headers to Verify

Always check whether the application returns:

- Content-Security-Policy
- Strict-Transport-Security
- X-Frame-Options
- X-Content-Type-Options
- Referrer-Policy
- Permissions-Policy

---

## Important Notes

- Sensitive information should never appear in URL query parameters.
- Session IDs should be transmitted using secure cookies instead of URLs.
- Review every HTTP header during web assessments.
- Hidden parameters and custom headers often reveal additional attack surfaces.

---

# 13. Quick Revision Sheet

URL Structure

```text
Scheme

↓

Host

↓

Port

↓

Path

↓

Query

↓

Fragment
```

---

Common URL Components

```text
https://example.com:443/login?id=5#profile
```

---

Common Request Headers

```text
Host

User-Agent

Authorization

Cookie

Accept

Content-Type

Origin

Referer
```

---

Common Response Headers

```text
Server

Content-Type

Content-Length

Set-Cookie

Location
```

---

Important Security Headers

```text
Content-Security-Policy

Strict-Transport-Security

X-Frame-Options

X-Content-Type-Options

Referrer-Policy

Permissions-Policy
```

---

Biggest Concept

```text
A URL identifies the location of
a web resource, while HTTP headers
provide additional information that
controls communication between the
client and the web server.

Understanding both is essential for
web development, API testing,
and web penetration testing.
```

---

*End of URL Format & HTTP Headers Notes*