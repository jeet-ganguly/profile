# Secure SDLC

## 📌 Quick Navigation

- [1. What is Secure SDLC?](#1-what-is-secure-sdlc)
  - [Why Secure SDLC is Introduced](#why-secure-sdlc-is-introduced)
  - [Traditional SDLC vs Secure SDLC](#traditional-sdlc-vs-secure-sdlc)
- [2. Secure SDLC Processes](#2-secure-sdlc-processes)
- [3. Risk Assessment](#3-risk-assessment)
  - [Types of Risk Assessment](#types-of-risk-assessment)
- [4. Threat Modeling](#4-threat-modeling)
  - [STRIDE](#stride)
  - [DREAD](#dread)
  - [PASTA](#pasta)
- [5. Secure Coding](#5-secure-coding)
  - [SAST](#sast)
  - [SCA](#sca)
  - [DAST](#dast)
  - [IAST](#iast)
  - [RASP](#rasp)
- [6. Security Assessment](#6-security-assessment)
  - [Vulnerability Assessment](#vulnerability-assessment)
  - [Penetration Testing](#penetration-testing)
  - [VA vs PT](#va-vs-pt)
- [7. SSDLC Methodologies](#7-ssdlc-methodologies)
  - [Microsoft SDL](#microsoft-sdl)
  - [OWASP Secure SDLC](#owasp-secure-sdlc)
- [SSDLC Security Flow](#ssdlc-security-flow)
- [Key Takeaways](#key-takeaways)


---

# 1. What is Secure SDLC?

**Secure SDLC (SSDLC) = Secure Software Development Life Cycle**

Secure SDLC is an approach that integrates **security activities into every phase of the software development lifecycle**.

Instead of testing security only after development:

```text
Traditional SDLC

Plan → Design → Code → Test → Deploy
                              ↓
                           Security
```

SSDLC integrates security throughout:

```text
Secure SDLC

Plan → Design → Code → Build → Test → Deploy → Operate
  ↓       ↓       ↓      ↓       ↓       ↓       ↓
 Risk   Threat   Secure  SAST   DAST    VA/PT   Monitor
Assess  Model    Coding   SCA    IAST
```

### Main Goal

> **Build security into the software from the beginning rather than fixing security problems at the end.**

---

## Why Secure SDLC is Introduced

Traditional development may discover security vulnerabilities late in the lifecycle.

### Problems with Late Security Testing

- Vulnerabilities discovered late
- Higher remediation cost
- Difficult architectural changes
- Production security incidents
- Delayed releases
- Security becomes a bottleneck

### SSDLC Benefits

- Early vulnerability detection
- Lower remediation cost
- Secure architecture
- Reduced attack surface
- Better compliance
- Improved software quality
- Continuous security

### Important Concept

```text
Security introduced early
        ↓
Early vulnerability detection
        ↓
Easier + cheaper remediation
        ↓
More secure application
```

---

# Traditional SDLC vs Secure SDLC

| Traditional SDLC | Secure SDLC |
|---|---|
| Security often added later | Security integrated from beginning |
| Security testing mainly near release | Security testing throughout lifecycle |
| Reactive security | Proactive security |
| Vulnerabilities discovered late | Vulnerabilities discovered early |
| Higher remediation cost | Lower remediation cost |
| Security may be separate | Security is shared responsibility |

---

# 2. Secure SDLC Processes

Security activities are mapped to each SDLC phase.

| SDLC Phase | Security Activities |
|---|---|
| Planning | Security requirements, risk assessment |
| Requirements | Security & compliance requirements |
| Design | Threat modeling, secure architecture |
| Development | Secure coding, code review, SAST, SCA |
| Testing | DAST, IAST, security testing |
| Deployment | Security configuration, VA/PT |
| Operations | Monitoring, patching, incident response |

### Complete Flow

```text
       PLANNING
          ↓
    Risk Assessment
          ↓
     REQUIREMENTS
          ↓
   Security Requirements
          ↓
        DESIGN
          ↓
    Threat Modeling
          ↓
     DEVELOPMENT
          ↓
 Secure Coding + SAST + SCA
          ↓
        TESTING
          ↓
 DAST + IAST + Security Testing
          ↓
      DEPLOYMENT
          ↓
      VA + PT
          ↓
      OPERATIONS
          ↓
Monitoring + Patching
          ↺
```

---

# 3. Risk Assessment

**Risk Assessment** is the process of identifying, analyzing and evaluating risks that could affect an application or organization.

### Basic Risk Concept

```text
Risk = Likelihood × Impact
```

Where:

- **Likelihood** → How likely the threat is to occur
- **Impact** → How much damage it could cause

### Example

```text
SQL Injection

Likelihood = High
Impact     = High

Risk = High
```

---

## Risk Assessment Process

```text
Identify Assets
      ↓
Identify Threats
      ↓
Identify Vulnerabilities
      ↓
Analyze Likelihood
      ↓
Analyze Impact
      ↓
Determine Risk
      ↓
Prioritize
      ↓
Mitigate / Accept / Transfer / Avoid
```

---

## Types of Risk Assessment

### 1. Qualitative Risk Assessment

Uses categories instead of numerical values.

Example:

```text
Likelihood → High
Impact     → High
Risk       → Critical
```

Common levels:

```text
Low → Medium → High → Critical
```

**Advantages:**

- Simple
- Fast
- Easy to communicate

---

### 2. Quantitative Risk Assessment

Uses numerical/financial values to estimate risk.

Example:

```text
Asset Value = $100,000
Probability of Loss = 10%

Estimated Risk = $10,000
```

Useful when organizations need to estimate **financial impact**.

---

### 3. Semi-Quantitative Risk Assessment

Combines qualitative categories with numerical scores.

Example:

```text
Likelihood = 4
Impact     = 5

Risk Score = 4 × 5 = 20
```

Then map the score:

```text
1–5   → Low
6–12  → Medium
13–20 → High
```

---

# 4. Threat Modeling

**Threat Modeling** is a structured process used to identify potential threats and design security controls before vulnerabilities are exploited.

It is commonly performed during the **design phase**.

### Basic Process

```text
Understand Application
        ↓
Identify Assets
        ↓
Identify Entry Points
        ↓
Identify Threats
        ↓
Analyze Risk
        ↓
Design Mitigations
```

### Threat Modeling Questions

```text
What are we building?
        ↓
What can go wrong?
        ↓
What can we do about it?
        ↓
Did we do a good job?
```

---

# STRIDE

**STRIDE** is a threat-modeling methodology developed by Microsoft.

| Letter | Threat | Meaning |
|---|---|---|
| **S** | Spoofing | Pretending to be another identity |
| **T** | Tampering | Unauthorized modification of data |
| **R** | Repudiation | Denying an action/event |
| **I** | Information Disclosure | Unauthorized access to information |
| **D** | Denial of Service | Making a service unavailable |
| **E** | Elevation of Privilege | Gaining unauthorized privileges |

### Easy Memory

```text
S → Spoofing
T → Tampering
R → Repudiation
I → Information Disclosure
D → Denial of Service
E → Elevation of Privilege
```

---

# DREAD

**DREAD** is a risk-rating model historically associated with Microsoft's threat modeling approach.

| Letter | Meaning |
|---|---|
| **D** | Damage Potential |
| **R** | Reproducibility |
| **E** | Exploitability |
| **A** | Affected Users |
| **D** | Discoverability |

It helps assign a **risk score** to a threat.

### Example

```text
Threat: SQL Injection

Damage Potential  → High
Reproducibility   → High
Exploitability    → High
Affected Users    → High
Discoverability   → Medium

                ↓

          High Risk
```

> **Note:** DREAD is mainly of historical/educational importance today; many modern teams prefer other risk-ranking approaches.

---

# PASTA

**PASTA = Process for Attack Simulation and Threat Analysis**

PASTA is a **risk-centric and attacker-focused threat modeling methodology**.

It connects:

```text
Business Requirements
        +
Application Architecture
        +
Threat Intelligence
        +
Attack Simulation
        ↓
Risk-Based Security Decisions
```

### 7 Stages of PASTA

| Stage | Purpose |
|---|---|
| **1** | Define business objectives |
| **2** | Define technical scope |
| **3** | Application decomposition |
| **4** | Threat analysis |
| **5** | Vulnerability analysis |
| **6** | Attack modeling |
| **7** | Risk & impact analysis |

### Key Idea

**STRIDE** → Categorizes threats  
**DREAD** → Rates threats  
**PASTA** → Performs risk-centric, attacker-focused threat analysis

---

# 5. Secure Coding

Secure coding means writing software in a way that prevents or reduces security vulnerabilities.

### Common Secure Coding Practices

- Input validation
- Output encoding
- Secure authentication
- Authorization checks
- Secure session management
- Proper error handling
- Secrets management
- Secure cryptography
- Dependency management
- Avoiding hardcoded credentials

### Security Testing Throughout Development

```text
Developer
    ↓
Code
    ↓
SAST ───────→ Source Code Analysis
    ↓
SCA ────────→ Dependency Analysis
    ↓
Build
    ↓
DAST ───────→ Running Application
    ↓
IAST ───────→ Runtime + Code Analysis
```

---

# SAST

**SAST = Static Application Security Testing**

Analyzes **source code, bytecode or binaries without executing the application**.

```text
Source Code
    ↓
   SAST
    ↓
Potential Vulnerabilities
```

### Finds Examples

- SQL Injection patterns
- XSS patterns
- Hardcoded secrets
- Insecure functions
- Weak cryptographic usage

### Key Point

> **SAST = Test the code without running the application.**

---

# SCA

**SCA = Software Composition Analysis**

Analyzes **third-party and open-source dependencies** used by an application.

```text
Application
    ↓
Dependencies
    ↓
SCA
    ↓
Known Vulnerabilities
```

### Finds

- Vulnerable libraries
- Outdated dependencies
- Known CVEs
- License issues
- Dependency risks

### Example

```text
Application
   ↓
log4j 2.x
   ↓
Known CVE
   ↓
SCA detects dependency risk
```

### Key Point

> **SCA = Secure the software components you depend on.**

---

# DAST

**DAST = Dynamic Application Security Testing**

Tests a **running application from the outside**.

```text
Running Web Application
          ↓
         DAST
          ↓
   HTTP Requests
          ↓
   Security Findings
```

It behaves similarly to an external attacker.

### Can Identify

- XSS
- SQL Injection
- Authentication issues
- Configuration issues
- Session problems

### Key Point

> **DAST = Test the running application.**

---

# IAST

**IAST = Interactive Application Security Testing**

Combines aspects of **SAST + DAST** by observing the application from inside while it is running.

```text
Running Application
        ↓
   IAST Agent
        ↓
Runtime + Code Information
        ↓
Security Finding
```

### Advantages

- Runtime visibility
- Code-level context
- Better vulnerability context
- Can reduce false positives compared with some purely external testing approaches

### Key Point

> **IAST = Runtime testing with internal application visibility.**

---

# RASP

**RASP = Runtime Application Self-Protection**

RASP operates **inside the running application** and can detect and potentially block malicious activity at runtime.

```text
Attacker
   ↓
Malicious Request
   ↓
Application
   ↓
  RASP
   ↓
Detect / Block
```

### Example

```text
Attacker
   ↓
SQL Injection Request
   ↓
Application
   ↓
RASP detects malicious behavior
   ↓
Request blocked
```

### Key Point

> **RASP = Protect the application while it is running.**

---

# SAST vs SCA vs DAST vs IAST vs RASP

| Technology | Main Focus | Runs Application? |
|---|---|---|
| **SAST** | Source/code | ❌ No |
| **SCA** | Dependencies/components | ❌ No |
| **DAST** | Running application from outside | ✅ Yes |
| **IAST** | Runtime + internal code visibility | ✅ Yes |
| **RASP** | Runtime protection | ✅ Yes |

### Easy Memory

```text
SAST → Source Code
SCA  → Components
DAST → Dynamic Application
IAST → Inside Application
RASP → Runtime Protection
```

---

# 6. Security Assessment

Security assessment evaluates whether an application has security weaknesses and whether existing security controls are effective.

Two important activities are:

```text
Security Assessment
       │
       ├── Vulnerability Assessment
       │
       └── Penetration Testing
```

---

# Vulnerability Assessment

**VA = Vulnerability Assessment**

A systematic process of identifying, analyzing and prioritizing vulnerabilities.

```text
Discover
   ↓
Scan
   ↓
Identify Vulnerabilities
   ↓
Validate
   ↓
Risk Rank
   ↓
Remediate
```

### Characteristics

- Broad coverage
- Often automated
- Identifies known vulnerabilities
- Prioritizes findings
- Usually less focused on exploitation

### Example

```text
Scanner
   ↓
Port 443
   ↓
Outdated TLS Configuration
   ↓
Medium Risk
```

---

# Penetration Testing

**PT = Penetration Testing**

An authorized security test where testers attempt to **exploit vulnerabilities** to determine their real-world impact.

```text
Recon
  ↓
Enumeration
  ↓
Vulnerability Identification
  ↓
Exploitation
  ↓
Privilege Escalation
  ↓
Impact
  ↓
Report
```

### Main Goal

> Determine whether vulnerabilities can actually be exploited and what impact they could have.

---

# VA vs PT

| Vulnerability Assessment | Penetration Testing |
|---|---|
| Finds vulnerabilities | Exploits vulnerabilities |
| Broad coverage | More targeted/deeper |
| Often automated | Manual + automated |
| Focuses on identification | Focuses on exploitation & impact |
| Produces vulnerability list | Demonstrates attack paths |
| Less intrusive generally | Can be more intrusive |

### Simple Difference

```text
VA:
"What vulnerabilities exist?"

PT:
"Can I exploit them, and what can I achieve?"
```

---

# 7. SSDLC Methodologies

Several organizations have developed frameworks and methodologies for implementing secure software development.

Two important examples:

- **Microsoft Security Development Lifecycle (SDL)**
- **OWASP Secure SDLC guidance**

---

# Microsoft SDL

**Microsoft Security Development Lifecycle (SDL)** is a security-focused software development methodology developed by Microsoft.

It integrates security practices throughout software development.

### Major Security Practices

```text
Training
   ↓
Security Requirements
   ↓
Threat Modeling
   ↓
Secure Design
   ↓
Secure Coding
   ↓
Security Testing
   ↓
Final Security Review
   ↓
Release
   ↓
Security Response
```

### Important Activities

- Security training
- Security requirements
- Threat modeling
- Attack surface analysis
- Secure coding
- Static analysis
- Dynamic analysis
- Security testing
- Final security review
- Incident/security response

### Key Idea

> Security is considered throughout development rather than being a final testing activity.

---

# OWASP Secure SDLC

**OWASP** provides guidance for integrating security into the software development lifecycle.

The approach emphasizes incorporating security activities into each phase.

### Example

```text
Requirements
     ↓
Security Requirements
     ↓
Design
     ↓
Threat Modeling
     ↓
Implementation
     ↓
Secure Coding + Code Review
     ↓
Testing
     ↓
SAST + DAST + SCA + Security Testing
     ↓
Deployment
     ↓
Secure Configuration
     ↓
Operations
     ↓
Monitoring + Incident Response
```

### Common OWASP Security Practices

- Security requirements
- Threat modeling
- Secure architecture
- Secure coding
- Code review
- Security testing
- Dependency management
- Vulnerability management
- Security monitoring
- Incident response

---

# Microsoft SDL vs OWASP Secure SDLC

| Microsoft SDL | OWASP Secure SDLC |
|---|---|
| Microsoft-developed methodology | OWASP security guidance/frameworks |
| Strongly integrated into Microsoft's development practices | Broadly applicable to web/software development |
| Security training | Security training |
| Security requirements | Security requirements |
| Threat modeling | Threat modeling |
| Secure coding | Secure coding |
| Security testing | Security testing |
| Final security review | Security verification/testing |
| Security response | Security monitoring/response |

---

# SSDLC Security Flow

The complete Secure SDLC can be remembered as:

```text
                    SECURE SDLC
                         │
 ┌───────────────────────┼────────────────────────┐
 ↓                       ↓                        ↓
Planning              Design                  Development
 ↓                       ↓                        ↓
Risk Assessment       Threat Modeling          Secure Coding
Security Requirements Secure Architecture      SAST
                                                 SCA
                         │
                         ↓
                      Testing
                         ↓
                  DAST / IAST
                         ↓
                   Security Assessment
                         ↓
                      VA / PT
                         ↓
                    Deployment
                         ↓
                Secure Configuration
                         ↓
                    Operations
                         ↓
             Monitoring / Response
                         ↺
```

---

# Quick Tool Mapping

| SDLC Stage | Security Activity |
|---|---|
| **Planning** | Risk Assessment |
| **Requirements** | Security Requirements |
| **Design** | Threat Modeling |
| **Development** | Secure Coding |
| **Code Analysis** | SAST |
| **Dependencies** | SCA |
| **Testing** | DAST / IAST |
| **Runtime** | RASP |
| **Pre-Production / Production Assessment** | VA / PT |
| **Operations** | Monitoring & Incident Response |

---

# Key Takeaways

- **SSDLC** integrates security into every phase of SDLC.
- Main objective: **find and fix security issues early**.
- **Risk Assessment** identifies and prioritizes security risks.
- Risk can be viewed as:

```text
Risk = Likelihood × Impact
```

- Common risk assessment types:
  - Qualitative
  - Quantitative
  - Semi-Quantitative
- **Threat Modeling** identifies threats before implementation.
- **STRIDE** → Threat categorization.
- **DREAD** → Historical risk-rating model.
- **PASTA** → Risk-centric, attacker-focused threat modeling.
- **SAST** → Source/code analysis.
- **SCA** → Third-party dependency analysis.
- **DAST** → Running application testing from outside.
- **IAST** → Runtime testing with internal application visibility.
- **RASP** → Runtime application protection.
- **VA** identifies vulnerabilities.
- **PT** attempts to exploit vulnerabilities and determine impact.
- **Microsoft SDL** provides Microsoft's security development methodology.
- **OWASP** provides widely used guidance for integrating security into SDLC.
- Core SSDLC philosophy:

```text
Security is not a final phase.

Security is integrated
throughout the entire SDLC.
```