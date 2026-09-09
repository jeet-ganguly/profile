# Introduction to DevSecOps

## 📌 Quick Navigation

- [1. What is DevOps?](#1-what-is-devops)
  - [Why DevOps is Needed](#why-devops-is-needed)
  - [DevOps Principles](#devops-principles)
- [2. Shifting Left](#2-shifting-left)
  - [Traditional Security Approach](#traditional-security-approach)
  - [Shift-Left Security](#shift-left-security)
- [3. DevSecOps Culture](#3-devsecops-culture)
  - [DevSecOps Principles](#devsecops-principles)
  - [Roles and Responsibilities](#roles-and-responsibilities)
- [DevOps vs DevSecOps](#devops-vs-devsecops)
- [Key Takeaways](#key-takeaways)


---

# 1. What is DevOps?

**DevOps = Development + Operations**

DevOps is a **culture and set of practices** that brings development and IT operations teams together to:

- Build software faster
- Test continuously
- Deploy frequently
- Automate repetitive tasks
- Improve collaboration
- Monitor applications continuously

### Simple Flow

```text
Develop
   ↓
Build
   ↓
Test
   ↓
Deploy
   ↓
Operate
   ↓
Monitor
   ↓
Feedback
   ↺
````

---

## Why DevOps is Needed

### Traditional Software Development

```text
Developer
   ↓
Write Code
   ↓
Throw code to Operations
   ↓
Operations deploys
   ↓
Problems discovered
   ↓
Back to Developer
```

This creates:

* Slow releases
* Poor communication
* Manual processes
* Deployment failures
* "Works on my machine" problems
* Difficult troubleshooting

### DevOps Approach

```text
Development  ←→  Operations
       ↓
   Automation
       ↓
 Continuous Integration
       ↓
 Continuous Delivery
       ↓
 Continuous Monitoring
```

DevOps aims to create a **faster and more reliable software delivery lifecycle**.

---

## DevOps Principles

### 1. Collaboration

Development and Operations work together instead of operating as isolated teams.

### 2. Automation

Automate repetitive tasks such as:

* Build
* Testing
* Deployment
* Infrastructure provisioning

### 3. Continuous Integration (CI)

Developers frequently merge code into a shared repository.

```text
Developer
   ↓
Commit Code
   ↓
Build
   ↓
Automated Tests
```

### 4. Continuous Delivery/Deployment (CD)

Automatically prepare or deploy tested software to environments.

### 5. Continuous Monitoring

Monitor:

* Application
* Infrastructure
* Performance
* Errors
* Availability
* Security events

---

# 2. Shifting Left

**Shifting Left** means moving a particular activity **earlier in the software development lifecycle**.

In DevSecOps, it mainly means:

> **Perform security testing and security checks earlier in development instead of waiting until deployment or production.**

---

## Traditional Security Approach

Security is often performed near the end:

```text
Plan → Code → Build → Test → Deploy → Security
                                      ↑
                                  Too Late
```

Problems:

* Vulnerabilities discovered late
* Expensive remediation
* Release delays
* Security becomes a bottleneck

---

## Shift-Left Security

Security activities are introduced much earlier:

```text
Plan
 ↓
Security Requirements
 ↓
Code
 ↓
Security Testing
 ↓
Build
 ↓
Security Scanning
 ↓
Deploy
 ↓
Monitor
```

### Example

Developer writes:

```python
query = "SELECT * FROM users WHERE name='" + username + "'"
```

A security tool can detect a potential **SQL Injection** issue during development/CI instead of discovering it after deployment.

---

## Why Shift Left?

| Late Security            | Shift Left                        |
| ------------------------ | --------------------------------- |
| Vulnerability found late | Vulnerability found early         |
| Higher remediation cost  | Lower remediation cost            |
| Security team bottleneck | Security distributed across teams |
| Manual testing           | Automated testing                 |
| Production-focused       | Lifecycle-focused                 |

### Important Idea

```text
Earlier Detection
       ↓
Lower Cost
       ↓
Faster Remediation
       ↓
More Secure Software
```

---

# 3. DevSecOps Culture

**DevSecOps = Development + Security + Operations**

DevSecOps extends DevOps by making **security a shared responsibility**.

```text
        DevSecOps
            │
    ┌───────┼───────┐
    ↓       ↓       ↓
Development Security Operations
    │       │       │
    └───────┼───────┘
            ↓
     Secure Software
```

### Core Principle

> **"Security is everyone's responsibility."**

Security should not be owned only by the security team.

---

## DevSecOps Principles

### 1. Shared Responsibility

Developers, security engineers and operations teams all contribute to security.

### 2. Security by Design

Security requirements are considered during **planning and architecture**, not only after development.

### 3. Automation

Security checks should be automated wherever possible.

Example:

```text
Code Commit
    ↓
Build
    ↓
SAST
    ↓
Dependency Scan
    ↓
Security Tests
    ↓
Deploy
```

### 4. Continuous Security

Security is integrated throughout the lifecycle:

```text
Plan → Code → Build → Test → Release → Deploy → Operate
  ↑      ↑      ↑       ↑       ↑        ↑        ↑
Security Security Security Security Security Security
```

### 5. Fast Feedback

Developers should receive security findings quickly so they can fix vulnerabilities while the code is still fresh.

### 6. Security as Code

Security policies and controls can be represented and automated as code.

Examples:

* Infrastructure security policies
* CI/CD security checks
* Infrastructure-as-Code scanning
* Compliance checks

---

## Roles and Responsibilities

### 👨‍💻 Developers

Responsible for:

* Secure coding
* Fixing vulnerabilities
* Following security requirements
* Reviewing security findings

### 🛡️ Security Team

Responsible for:

* Security architecture
* Security standards
* Threat modeling
* Security tooling
* Vulnerability management
* Security guidance

### ⚙️ Operations

Responsible for:

* Secure infrastructure
* Access control
* Monitoring
* Logging
* Secure deployment
* Infrastructure security

### Shared Responsibility

```text
Developer
   ↓
Secure Code

Security
   ↓
Security Controls

Operations
   ↓
Secure Infrastructure

        ↓

   DevSecOps
        ↓
Secure Application
```

---

# DevOps vs DevSecOps

| DevOps                         | DevSecOps                                |
| ------------------------------ | ---------------------------------------- |
| Development + Operations       | Development + Security + Operations      |
| Focus on speed and reliability | Focus on speed, reliability and security |
| Security may be separate       | Security is integrated                   |
| Security often added later     | Security starts early                    |
| Security team may be a gate    | Security is shared responsibility        |
| Automation                     | Security automation + automation         |

---

# Key Takeaways

* **DevOps** combines Development and Operations to improve software delivery.
* DevOps focuses heavily on **collaboration, automation and continuous delivery**.
* **Shifting Left** means moving security/testing activities earlier in the SDLC.
* Finding vulnerabilities early is generally **faster and cheaper to fix**.
* **DevSecOps** integrates security into the DevOps lifecycle.
* DevSecOps is not just about security tools; it is primarily a **culture and shared responsibility model**.
* Developers, Security and Operations all participate in application security.
* The goal is:

```text
Build Fast
   +
Build Reliable
   +
Build Secure
   ↓
DevSecOps
```