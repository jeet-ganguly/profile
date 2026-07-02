# Computer Networks — HTTP & HTTP Methods

> **HTTP (HyperText Transfer Protocol)** is the foundation of communication on the World Wide Web. It defines how web clients (browsers) and web servers exchange data.
>
> Every time you open a website, submit a login form, download a file, or access an API, HTTP is used to transfer the request and response between the client and server.

These notes cover:

- What is HTTP?
- Why HTTP is Needed
- HTTP Architecture
- HTTP Communication Process
- HTTP Message Format
- HTTP Request
- HTTP Response
- HTTP Methods
- Safe vs Unsafe Methods
- Idempotent Methods
- Common HTTP Status Codes
- Cybersecurity Perspective
- Quick Revision Sheet

---

# Table of Contents

- [1. What is HTTP?](#1-what-is-http)
- [2. Why HTTP is Needed](#2-why-http-is-needed)
- [3. HTTP Architecture](#3-http-architecture)
- [4. HTTP Communication Process](#4-http-communication-process)
- [5. HTTP Messages](#5-http-messages)
- [6. HTTP Request](#6-http-request)
- [7. HTTP Response](#7-http-response)
- [8. HTTP Methods](#8-http-methods)
- [9. Safe vs Unsafe Methods](#9-safe-vs-unsafe-methods)
- [10. Idempotent Methods](#10-idempotent-methods)
- [11. Common HTTP Status Codes](#11-common-http-status-codes)
- [12. Cybersecurity Perspective](#12-cybersecurity-perspective)
- [13. Quick Revision Sheet](#13-quick-revision-sheet)

---

# 1. What is HTTP?

HTTP stands for:

```text
HyperText Transfer Protocol
```

It is an **Application Layer** protocol used to transfer web pages and other resources between a client and a web server.

HTTP follows a **Client-Server Architecture**.

---

## Example

```text
Browser

↓

HTTP Request

↓

Web Server

↓

HTTP Response

↓

Browser Displays Website
```

---

## Characteristics of HTTP

- Application Layer Protocol
- Client-Server Model
- Request-Response Protocol
- Stateless Protocol
- Human Readable
- Extensible using Headers

---

# 2. Why HTTP is Needed

Browsers and servers need a common language to communicate.

HTTP defines:

- How requests are sent
- How responses are returned
- How resources are identified
- How errors are reported

Without HTTP, browsers could not communicate with web servers.

---

## Example

User enters:

```text
https://example.com
```

Browser sends:

```text
HTTP Request
```

Server replies:

```text
HTML Page
```

Browser renders the webpage.

---

# 3. HTTP Architecture

HTTP uses a simple Client-Server architecture.

```text
+-----------+                  +-------------+
|   Client  |                  | Web Server  |
| (Browser) |                  | (Apache)    |
+-----------+                  +-------------+
      │                               │
      │ HTTP Request                  │
      ├──────────────────────────────►│
      │                               │
      │ HTTP Response                 │
      ◄───────────────────────────────┤
      │                               │
```

---

## Components

### Client

The client requests a resource.

Examples:

- Chrome
- Firefox
- Edge
- curl
- Burp Suite

---

### Server

The server processes requests and returns responses.

Examples:

- Apache
- Nginx
- IIS
- Node.js
- Tomcat

---

# 4. HTTP Communication Process

Whenever a webpage is opened, the following process occurs.

```text
User

↓

Browser

↓

DNS Resolution

↓

TCP Connection

↓

HTTP Request

↓

Web Server

↓

HTTP Response

↓

Browser Renders Page
```

---

## Detailed Steps

### Step 1

User enters:

```text
https://example.com
```

---

### Step 2

DNS resolves:

```text
example.com

↓

192.168.1.10
```

---

### Step 3

Browser establishes a TCP connection.

---

### Step 4

Browser sends an HTTP request.

---

### Step 5

Server processes the request.

---

### Step 6

Server returns an HTTP response.

---

### Step 7

Browser displays the webpage.

---

# 5. HTTP Messages

HTTP communication consists of two message types.

```text
Client

↓

HTTP Request

↓

Server

↓

HTTP Response
```

---

## Request Message

Sent by the client.

Contains:

- Request Line
- Headers
- Body (optional)

---

## Response Message

Sent by the server.

Contains:

- Status Line
- Headers
- Body

---

# 6. HTTP Request

Example:

```http
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
Connection: close
```

---

## Request Components

### Request Line

```text
GET /index.html HTTP/1.1
```

Contains:

- Method
- Resource
- HTTP Version

---

### Headers

Examples:

```text
Host

User-Agent

Accept

Authorization

Cookie

Content-Type
```

Headers provide additional information to the server.

---

### Request Body

Used mainly with:

- POST
- PUT
- PATCH

Example:

```json
{
  "username":"admin",
  "password":"password"
}
```

---

# 7. HTTP Response

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 3500

<html>
...
</html>
```

---

## Response Components

### Status Line

```text
HTTP/1.1 200 OK
```

Contains:

- HTTP Version
- Status Code
- Status Message

---

### Response Headers

Examples:

```text
Server

Content-Type

Set-Cookie

Content-Length

Location
```

---

### Response Body

Contains:

- HTML
- JSON
- Images
- CSS
- JavaScript

---

# 8. HTTP Methods

HTTP methods define the action that the client wants the server to perform.

---

## GET

Retrieves data from the server.

Example:

```http
GET /products HTTP/1.1
```

Common Uses:

- Open webpages
- Retrieve API data
- Download files

---

## POST

Submits data to the server.

Example:

```http
POST /login HTTP/1.1
```

Common Uses:

- Login
- Registration
- Form Submission
- File Upload

---

## PUT

Replaces an existing resource.

Example:

```http
PUT /users/10 HTTP/1.1
```

Used for:

- Full Update
- Replace Existing Data

---

## PATCH

Updates part of an existing resource.

Example:

```http
PATCH /users/10 HTTP/1.1
```

Used for:

- Partial Update

Example:

Only changing:

```text
Email Address
```

instead of replacing the whole user.

---

## DELETE

Deletes an existing resource.

Example:

```http
DELETE /users/10 HTTP/1.1
```

Used for:

- Remove Files
- Delete User Accounts
- Delete Records

---

## HEAD

Similar to GET but returns **only headers**.

Example:

```http
HEAD /index.html HTTP/1.1
```

Used for:

- Check Server Status
- View Response Headers
- Verify File Exists

---

## OPTIONS

Returns the HTTP methods supported by the server.

Example:

```http
OPTIONS /api HTTP/1.1
```

Typical Response:

```text
Allow:

GET

POST

PUT
```

Useful during API testing.

---

## TRACE

Returns the received HTTP request back to the client.

Primarily used for:

- Diagnostics
- Debugging

Often disabled due to security risks.

---

## CONNECT

Creates a tunnel between the client and the server.

Commonly used by:

- HTTPS Proxies
- VPN Proxies

---

## Summary

| Method | Purpose |
|---------|----------|
| GET | Retrieve Data |
| POST | Create Resource / Submit Data |
| PUT | Replace Resource |
| PATCH | Partial Update |
| DELETE | Delete Resource |
| HEAD | Headers Only |
| OPTIONS | Supported Methods |
| TRACE | Debugging |
| CONNECT | Proxy Tunnel |

---

# 9. Safe vs Unsafe Methods

## Safe Methods

Do not modify server data.

| Method |
|---------|
| GET |
| HEAD |
| OPTIONS |

---

## Unsafe Methods

Modify server resources.

| Method |
|---------|
| POST |
| PUT |
| PATCH |
| DELETE |

---

# 10. Idempotent Methods

An idempotent method produces the same result even if executed multiple times.

| Method | Idempotent |
|----------|------------|
| GET | Yes |
| PUT | Yes |
| DELETE | Yes |
| HEAD | Yes |
| OPTIONS | Yes |
| POST | No |
| PATCH | Usually No |

---

## Example

PUT

```text
Update Age = 25
```

Running it ten times:

```text
Age remains 25
```

---

POST

```text
Create New User
```

Running it ten times:

```text
Creates 10 Users
```

---

# 11. Common HTTP Status Codes

| Code | Meaning |
|------|----------|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 301 | Permanent Redirect |
| 302 | Temporary Redirect |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |

---

# 12. Cybersecurity Perspective

HTTP is one of the primary attack surfaces during web penetration testing.

---

## Important Methods to Test

Always test:

- GET
- POST
- PUT
- PATCH
- DELETE
- OPTIONS

Some APIs unintentionally expose dangerous methods.

---

## Misconfigured HTTP Methods

Look for:

```text
PUT

DELETE

TRACE
```

These may allow unauthorized actions.

---

## Inspect Headers

Review response headers for:

- Server
- X-Powered-By
- Content-Security-Policy
- Strict-Transport-Security
- X-Frame-Options
- X-Content-Type-Options

These reveal technologies and security configurations.

---

## Authentication Testing

Test whether:

- Sensitive endpoints require authentication.
- Authorization is properly enforced.
- Cookies and tokens are validated.

---

## Common Web Vulnerabilities

HTTP requests are commonly used to test for:

- SQL Injection
- Cross-Site Scripting (XSS)
- IDOR
- Command Injection
- File Upload Vulnerabilities
- CSRF
- SSRF
- Broken Authentication

---

## Important Notes

- Never assume unsupported methods are disabled—verify using `OPTIONS`.
- Examine every request and response header during testing.
- Compare authenticated and unauthenticated requests.
- Understand the application's API before attempting exploitation.

---

# 13. Quick Revision Sheet

HTTP

```text
HyperText Transfer Protocol
```

---

Architecture

```text
Client

↓

HTTP Request

↓

Server

↓

HTTP Response
```

---

Safe Methods

```text
GET

HEAD

OPTIONS
```

---

Unsafe Methods

```text
POST

PUT

PATCH

DELETE
```

---

Common Methods

```text
GET

POST

PUT

PATCH

DELETE

HEAD

OPTIONS
```

---

Common Status Codes

```text
200 OK

301 Redirect

400 Bad Request

401 Unauthorized

403 Forbidden

404 Not Found

500 Internal Server Error
```

---

Biggest Concept

```text
HTTP is a stateless
Application Layer protocol
that enables communication
between clients and web servers
using request-response messages
and standardized HTTP methods.
```

---

*End of HTTP & HTTP Methods Notes*