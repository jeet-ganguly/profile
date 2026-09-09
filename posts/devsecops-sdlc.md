# Introduction to SDLC

## 📌 Quick Navigation

- [1. What is SDLC?](#1-what-is-sdlc)
  - [Why SDLC is Needed](#why-sdlc-is-needed)
- [2. SDLC Phases](#2-sdlc-phases)
  - [1. Planning](#1-planning)
  - [2. Requirements Analysis](#2-requirements-analysis)
  - [3. Design](#3-design)
  - [4. Development](#4-development)
  - [5. Testing](#5-testing)
  - [6. Deployment](#6-deployment)
  - [7. Maintenance](#7-maintenance)
- [3. CALMS](#3-calms)
  - [Culture](#culture)
  - [Automation](#automation)
  - [Lean](#lean)
  - [Measurement](#measurement)
  - [Sharing](#sharing)
- [4. DevOps Metrics](#4-devops-metrics)
  - [Deployment Frequency](#deployment-frequency)
  - [Lead Time for Changes](#lead-time-for-changes)
  - [Change Failure Rate](#change-failure-rate)
  - [Mean Time to Recovery](#mean-time-to-recovery)
  - [Other Useful Metrics](#other-useful-metrics)
- [SDLC + DevOps](#sdlc--devops)
- [Key Takeaways](#key-takeaways)


---

# 1. What is SDLC?

**SDLC = Software Development Life Cycle**

SDLC is a **structured process used to plan, develop, test, deploy and maintain software**.

It defines **how software moves from an idea to a working product and eventually maintenance**.

### Simple Flow

```text
Planning
   ↓
Requirements
   ↓
Design
   ↓
Development
   ↓
Testing
   ↓
Deployment
   ↓
Maintenance
   ↺
```

---

## Why SDLC is Needed

Without a structured development process:

- Requirements may be unclear
- Development becomes difficult to manage
- Bugs may be discovered late
- Security issues may be missed
- Projects can exceed time/budget
- Maintenance becomes difficult

SDLC provides:

- Structured development
- Better project management
- Defined responsibilities
- Quality control
- Testing
- Documentation
- Maintainability

---

# 2. SDLC Phases

## 1. Planning

Determine:

- What needs to be built?
- Why is it needed?
- Project scope
- Resources
- Timeline
- Cost
- Risks

```text
Idea
 ↓
Feasibility
 ↓
Project Plan
```

---

## 2. Requirements Analysis

Identify and document what the software must do.

### Types of Requirements

**Functional Requirements**

What the system should do.

Example:

```text
User should be able to reset password.
```

**Non-Functional Requirements**

How the system should perform.

Examples:

- Performance
- Availability
- Scalability
- Security
- Reliability

### Security Example

Instead of:

```text
"User can login."
```

A security requirement could be:

```text
"User authentication must use MFA."
```

---

## 3. Design

Define **how the software will be built**.

Includes:

- Architecture
- Database design
- APIs
- Components
- UI design
- Authentication
- Security controls

### Example

```text
              Application
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
     Frontend    API       Database
                   │
              Authentication
```

Security activities such as **threat modeling** can be performed during this phase.

---

## 4. Development

Developers convert the design and requirements into code.

Activities:

- Write code
- Code review
- Version control
- Unit testing
- Dependency management

Example:

```text
Developer
    ↓
Write Code
    ↓
Git Commit
    ↓
Code Review
```

---

## 5. Testing

Verify that the software works as expected.

### Common Testing

| Testing | Purpose |
|---|---|
| Unit Testing | Test individual components |
| Integration Testing | Test components together |
| System Testing | Test complete system |
| Regression Testing | Ensure changes don't break existing functionality |
| Performance Testing | Test performance/load |
| Security Testing | Find security vulnerabilities |
| Acceptance Testing | Validate business requirements |

### Security Testing Examples

- SAST
- DAST
- Dependency scanning
- Secret scanning
- Penetration testing

---

## 6. Deployment

Move the application into an environment where users can access it.

```text
Development
     ↓
Testing
     ↓
Staging
     ↓
Production
```

Deployment can be:

- Manual
- Automated
- Continuous Deployment

DevOps commonly uses **CI/CD pipelines** to automate this process.

---

## 7. Maintenance

After deployment, the software must continuously be maintained.

Activities:

- Bug fixing
- Security patching
- Performance improvements
- Feature updates
- Monitoring
- Incident response

```text
Production
    ↓
Monitor
    ↓
Issue / Feedback
    ↓
Fix / Improve
    ↓
Deploy
    ↺
```

---

# 3. CALMS

**CALMS** is a framework used to assess and implement **DevOps culture and practices**.

```text
        CALMS
          │
 ┌────────┼────────┐
 ↓        ↓        ↓
Culture Automation Lean
 │        │        │
 └────────┼────────┘
          ↓
     Measurement
          ↓
       Sharing
```

**C = Culture**  
**A = Automation**  
**L = Lean**  
**M = Measurement**  
**S = Sharing**

---

## Culture

Focuses on collaboration and shared responsibility.

### Key Ideas

- Break team silos
- Collaboration
- Shared ownership
- Continuous improvement
- Trust

Example:

```text
Development ←→ Security ←→ Operations
```

This is especially important in **DevSecOps**.

---

## Automation

Automate repetitive and error-prone tasks.

Examples:

- Build
- Testing
- Deployment
- Infrastructure provisioning
- Security scanning

```text
Code
 ↓
Build
 ↓
Test
 ↓
Security Scan
 ↓
Deploy
```

Automation improves:

- Speed
- Consistency
- Reliability
- Scalability

---

## Lean

Lean focuses on **reducing waste and improving flow**.

### Waste Examples

- Waiting
- Manual repetitive work
- Unnecessary processes
- Rework
- Large batches of changes

### Goal

```text
Small Changes
     ↓
Fast Feedback
     ↓
Continuous Improvement
```

---

## Measurement

Measure the performance of the development and delivery process.

Examples:

- Deployment frequency
- Lead time
- Failure rate
- Recovery time
- Build time

> **You cannot effectively improve what you do not measure.**

---

## Sharing

Encourage knowledge and information sharing.

Examples:

- Documentation
- Team discussions
- Post-incident reviews
- Code reviews
- Shared dashboards
- Lessons learned

Goal:

```text
Knowledge
   ↓
Shared Across Teams
   ↓
Better Collaboration
   ↓
Continuous Improvement
```

---

# 4. DevOps Metrics

**DevOps metrics** measure the efficiency, speed, reliability and quality of software delivery.

The most important metrics are commonly known as the **DORA metrics**.

---

## Deployment Frequency

Measures **how frequently an organization successfully deploys changes to production**.

```text
More frequent successful deployments
                ↓
        Faster delivery
```

Example:

```text
Team A → 2 deployments/month
Team B → 20 deployments/month
```

Team B has a higher deployment frequency.

---

## Lead Time for Changes

Measures the time between **a code change being committed and that change successfully running in production**.

```text
Code Commit
    ↓
Build
    ↓
Test
    ↓
Deploy
    ↓
Production
```

Shorter lead time generally indicates a faster delivery process.

---

## Change Failure Rate

Measures the **percentage of deployments that result in a failure requiring remediation**, such as a rollback, hotfix or similar corrective action.

### Formula

```text
Change Failure Rate =
Failed Changes / Total Changes × 100
```

Example:

```text
100 deployments
20 caused failures

Change Failure Rate = 20%
```

Lower is generally better.

---

## Mean Time to Recovery (MTTR)

Measures how quickly a service is restored after a failure.

```text
Failure
   ↓
Detection
   ↓
Investigation
   ↓
Fix
   ↓
Recovery
```

### Example

```text
Incident occurs: 10:00
Service restored: 10:30

MTTR = 30 minutes
```

Lower MTTR generally indicates faster recovery.

---

## Other Useful Metrics

### Build Success Rate

Percentage of builds that successfully complete.

```text
Successful Builds
----------------- × 100
Total Builds
```

### Test Pass Rate

Percentage of automated tests that pass.

### Deployment Duration

Time required to complete a deployment.

### Mean Time to Detect (MTTD)

Time taken to detect an incident after it occurs.

```text
Incident
   ↓
Detection
```

Lower MTTD = faster detection.

---

# SDLC + DevOps

Traditional SDLC describes **the software lifecycle**, while DevOps focuses on **improving the flow of development, delivery and operations across that lifecycle**.

```text
             SDLC
              │
 ┌────────────┼────────────┐
 ↓            ↓            ↓
Plan         Build        Operate
 │            │            │
Requirements Development  Monitor
 │            │            │
Design       Testing      Feedback
 └────────────┼────────────┘
              ↓
       DevOps Practices
              ↓
    Automation + CI/CD
              ↓
      Faster Feedback
              ↓
     Continuous Delivery
```

### DevSecOps Extension

Security is integrated throughout the SDLC:

```text
Plan → Design → Code → Build → Test → Deploy → Operate
  ↓      ↓       ↓      ↓       ↓       ↓       ↓
Security Security Security Security Security Security Security
```

---

# Key Takeaways

- **SDLC** is a structured process for developing and maintaining software.
- Main SDLC phases are:
  **Planning → Requirements → Design → Development → Testing → Deployment → Maintenance**
- **CALMS** represents:
  - **C** → Culture
  - **A** → Automation
  - **L** → Lean
  - **M** → Measurement
  - **S** → Sharing
- DevOps metrics help measure **speed, reliability and delivery performance**.
- The four key **DORA metrics** are:
  - Deployment Frequency
  - Lead Time for Changes
  - Change Failure Rate
  - Mean Time to Recovery
- **DevOps improves the SDLC through collaboration, automation, measurement and continuous feedback.**
- **DevSecOps adds security throughout the SDLC rather than treating security as a final step.**
