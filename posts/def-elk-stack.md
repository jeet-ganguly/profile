# Working of ELK Stack for SIEM

> The **ELK Stack** is a collection of tools used to collect, process, search, analyze, and visualize logs. In a SIEM environment, it can be used to centralize security logs from multiple systems and help security analysts detect and investigate suspicious activity.

> **ELK** traditionally stands for **Elasticsearch, Logstash, and Kibana**. In modern Elastic deployments, **Beats/Elastic Agent** are commonly used for log collection and forwarding.

These notes cover:

* What is ELK Stack
* What is SIEM
* Components of ELK
* Elasticsearch
* Logstash
* Kibana
* Beats / Elastic Agent
* Working of ELK as SIEM
* Log Processing Pipeline
* Example Security Event Flow
* Detection and Investigation
* ELK vs Traditional SIEM
* Cybersecurity Perspective
* Quick Revision Sheet

---

# Table of Contents

* [1. What is ELK Stack](#1-what-is-elk-stack)
* [2. What is SIEM](#2-what-is-siem)
* [3. ELK Stack Components](#3-elk-stack-components)
* [4. Elasticsearch](#4-elasticsearch)
* [5. Logstash](#5-logstash)
* [6. Kibana](#6-kibana)
* [7. Beats and Elastic Agent](#7-beats-and-elastic-agent)
* [8. Working of ELK as SIEM](#8-working-of-elk-as-siem)
* [9. Log Processing Pipeline](#9-log-processing-pipeline)
* [10. Example Security Event Flow](#10-example-security-event-flow)
* [11. Detection and Investigation](#11-detection-and-investigation)
* [12. ELK in a SOC](#12-elk-in-a-soc)
* [13. ELK vs Traditional SIEM](#13-elk-vs-traditional-siem)
* [14. Cybersecurity Perspective](#14-cybersecurity-perspective)
* [15. Quick Revision Sheet](#15-quick-revision-sheet)

---

# 1. What is ELK Stack

ELK is a log management and analytics stack consisting of:

```text
Elasticsearch
+
Logstash
+
Kibana
```

A typical security architecture may also include:

```text
Elastic Agent / Beats
```

for collecting logs.

The basic idea is:

```text
Collect Logs

↓

Process Logs

↓

Store & Search Logs

↓

Visualize Logs

↓

Detect & Investigate Threats
```

---

# 2. What is SIEM

**SIEM** stands for:

```text
Security Information and Event Management
```

A SIEM collects security events from different sources and provides a centralized platform for:

* Log Collection
* Event Analysis
* Correlation
* Threat Detection
* Alerting
* Investigation
* Incident Response

---

## Example

An organization may have:

```text
Windows Servers
Linux Servers
Firewalls
Web Servers
VPN
Endpoints
Applications
```

Each system generates logs.

Instead of checking every system separately:

```text
Multiple Log Sources

        ↓

    Central SIEM

        ↓

Security Monitoring
```

---

# 3. ELK Stack Components

The basic architecture is:

```text
Log Sources

↓

Log Collection

↓

Log Processing

↓

Elasticsearch

↓

Kibana

↓

Security Analysis
```

---

## Main Components

| Component                 | Main Purpose                                          |
| ------------------------- | ----------------------------------------------------- |
| **Elastic Agent / Beats** | Collect and forward logs                              |
| **Logstash**              | Process, filter, transform, and route logs            |
| **Elasticsearch**         | Store, index, and search logs                         |
| **Kibana**                | Visualize, search, investigate, and create dashboards |

---

# 4. Elasticsearch

**Elasticsearch** is the search and analytics engine of the stack.

It stores and indexes collected events so that they can be searched efficiently.

---

## Main Functions

Elasticsearch provides:

```text
Log Storage

Indexing

Searching

Filtering

Aggregation

Analytics
```

---

## Example

Suppose Elasticsearch receives:

```text
Windows Event ID 4625

Source IP: 10.10.10.20

Username: admin
```

The event can be indexed and later searched using:

```text
Event ID

Username

Source IP

Timestamp
```

---

## Basic Concept

```text
Logs

↓

Elasticsearch

↓

Indexed Data

↓

Fast Search
```

---

# 5. Logstash

**Logstash** is a data processing pipeline.

It can receive data from different sources, process it, and send it to a destination.

The basic Logstash pipeline is:

```text
Input

↓

Filter

↓

Output
```

---

## Input

Logstash receives data from sources such as:

```text
Log Files

Network Connections

Beats

Applications

Other Data Sources
```

---

## Filter

The filter stage can:

```text
Parse Logs

Extract Fields

Modify Data

Normalize Data

Remove Unwanted Data

Add Fields
```

---

## Output

Processed data can be sent to:

```text
Elasticsearch

Other Destinations
```

---

## Example

Raw log:

```text
Failed login from 10.10.10.20
```

Logstash can transform it into structured fields:

```text
event.action = failed-login

source.ip = 10.10.10.20

event.category = authentication
```

This makes searching and detection easier.

---

# 6. Kibana

**Kibana** provides the visualization and investigation interface.

It allows analysts to:

* Search Logs
* Create Dashboards
* Visualize Events
* Investigate Alerts
* Analyze Trends
* Explore Security Data

---

## Example Dashboard

A SOC dashboard may show:

```text
Failed Logins

Successful Logins

Top Source IPs

Top Destination IPs

Suspicious Processes

DNS Requests

Security Alerts
```

---

## Basic Concept

```text
Elasticsearch

↓

Kibana

↓

Dashboard

↓

SOC Analyst
```

---

# 7. Beats and Elastic Agent

Logs must first be collected from endpoints and systems.

Two common approaches are:

```text
Beats

Elastic Agent
```

---

## Beats

Beats are lightweight data shippers.

Examples include:

```text
Filebeat
Metricbeat
Winlogbeat
Packetbeat
```

### Winlogbeat

Used for collecting Windows Event Logs.

Example:

```text
Windows Event Log

↓

Winlogbeat

↓

Elasticsearch / Logstash
```

---

## Elastic Agent

Elastic Agent provides a more unified approach for collecting:

```text
Logs

Metrics

Security Telemetry

Endpoint Data
```

It can simplify endpoint data collection compared with managing multiple individual Beats.

---

# 8. Working of ELK as SIEM

A typical ELK-based SIEM architecture can be represented as:

```text
+----------------------+
|     Log Sources      |
|                      |
| Windows              |
| Linux                |
| Firewall             |
| VPN                  |
| Web Server           |
| Endpoint              |
+----------+-----------+
           |
           ↓
+----------------------+
| Elastic Agent /      |
| Beats / Collectors   |
+----------+-----------+
           |
           ↓
+----------------------+
|      Logstash        |
|                      |
| Input                |
| Filter               |
| Parsing              |
| Normalization        |
+----------+-----------+
           |
           ↓
+----------------------+
|    Elasticsearch     |
|                      |
| Store                |
| Index                |
| Search               |
+----------+-----------+
           |
           ↓
+----------------------+
|       Kibana         |
|                      |
| Search               |
| Dashboard            |
| Visualization        |
| Investigation        |
+----------+-----------+
           |
           ↓
+----------------------+
|     SOC Analyst      |
+----------------------+
```

---

# 9. Log Processing Pipeline

The complete process can be understood in several stages.

---

## Step 1 — Log Generation

Devices and applications generate logs.

Example:

```text
Windows

↓

Failed Login
```

---

## Step 2 — Log Collection

An agent collects the event.

```text
Windows Event Log

↓

Winlogbeat / Elastic Agent
```

---

## Step 3 — Log Forwarding

The collected event is forwarded to the processing or storage layer.

```text
Agent

↓

Logstash
```

or in some architectures:

```text
Agent

↓

Elasticsearch
```

---

## Step 4 — Log Processing

Logstash can parse and normalize the event.

```text
Raw Log

↓

Parsing

↓

Structured Fields
```

Example:

```text
Username

Source IP

Event ID

Timestamp

Host
```

---

## Step 5 — Storage and Indexing

Elasticsearch stores and indexes the event.

```text
Processed Event

↓

Elasticsearch

↓

Index
```

---

## Step 6 — Search and Visualization

Kibana queries Elasticsearch.

```text
Elasticsearch

↓

Kibana

↓

Dashboard / Search
```

---

## Step 7 — Detection

Security detection rules can identify suspicious behavior.

```text
Events

↓

Detection Rule

↓

Match

↓

Alert
```

---

## Step 8 — Investigation

The SOC analyst investigates:

```text
Who?

What?

When?

Where?

How?

What happened before?

What happened after?
```

---

# 10. Example Security Event Flow

Suppose an attacker repeatedly attempts to log in to a Windows system.

The Windows machine generates:

```text
Event ID 4625

↓

Failed Logon
```

---

## Collection

```text
Windows

↓

Winlogbeat / Elastic Agent

↓

Logstash
```

---

## Processing

Logstash extracts fields such as:

```text
Event ID

Username

Source IP

Timestamp

Hostname
```

---

## Elasticsearch

The structured event is indexed:

```text
Elasticsearch

↓

Searchable Event
```

---

## Detection

Suppose the system observes:

```text
4625
4625
4625
4625
4625
```

from the same source IP.

A detection rule can identify:

```text
Multiple Failed Logons

↓

Potential Brute Force
```

---

## Kibana

The SOC analyst can see:

```text
Source IP

Username

Number of Attempts

Target Host

Time Range
```

and investigate the activity.

---

# 11. Detection and Investigation

A SIEM is more than a log storage system.

The important process is:

```text
Collect

↓

Normalize

↓

Search

↓

Correlate

↓

Detect

↓

Alert

↓

Investigate
```

---

## Example Correlation

Consider:

```text
4625

↓

Multiple Failed Logons

↓

4624

↓

Successful Logon

↓

4688

↓

Suspicious Process

↓

Sysmon 3

↓

Network Connection
```

A SIEM can correlate these events to provide a stronger indication of compromise.

---

## Detection Rule

A simplified detection could be:

```text
IF

Multiple failed logins

+

Successful login

+

Suspicious process

↓

Generate Alert
```

---

# 12. ELK in a SOC

ELK can support several SOC activities.

---

## Monitoring

Analysts can monitor:

```text
Authentication

Network Activity

Endpoint Activity

System Events

Application Logs
```

---

## Threat Hunting

Analysts can search for:

```text
Suspicious IPs

Suspicious Domains

Unusual Users

Failed Logins

PowerShell Activity

Process Execution
```

---

## Incident Investigation

Analysts can build timelines:

```text
Initial Login

↓

Process Execution

↓

Network Connection

↓

Persistence

↓

Data Access
```

---

## Dashboards

A SOC dashboard may contain:

```text
Failed Login Count

Top Source IPs

Top Users

Top Alerts

Network Connections

Endpoint Events

DNS Requests
```

---

# 13. ELK vs Traditional SIEM

ELK can provide many SIEM capabilities, but **ELK by itself is not automatically a complete SIEM**.

A SIEM requires capabilities such as:

```text
Data Collection

Detection

Correlation

Alerting

Investigation

Visualization
```

A modern Elastic security deployment combines these capabilities through the broader Elastic Security platform.

---

## Important Concept

```text
ELK

=

Elasticsearch

+

Logstash

+

Kibana
```

Whereas:

```text
SIEM

=

Logs

+

Detection

+

Correlation

+

Alerting

+

Investigation
```

Therefore:

```text
ELK

↓

Can form the foundation of
a SIEM architecture
```

---

# 14. Cybersecurity Perspective

ELK is useful for centralized security monitoring.

---

## Windows Threat Hunting

Collect:

```text
Windows Event Logs

Sysmon

PowerShell Logs
```

Then:

```text
Agent

↓

Logstash

↓

Elasticsearch

↓

Kibana

↓

Threat Hunting
```

---

## Network Security

Collect:

```text
Firewall Logs

VPN Logs

DNS Logs

Proxy Logs

Network Telemetry
```

Then search for:

```text
Suspicious IPs

Unusual Connections

Repeated Connections

Potential C2 Activity
```

---

## Authentication Monitoring

Monitor:

```text
4624

4625

4672

4740

4768

4769
```

Look for:

```text
Brute Force

Password Spraying

Unusual Logins

Privileged Logons

Kerberos Anomalies
```

---

## Incident Response

ELK can help analysts answer:

```text
What happened?

When did it happen?

Which host was affected?

Which user was involved?

What process executed?

What IP communicated with the host?

What happened before and after the event?
```

---

# 15. Quick Revision Sheet

## ELK Components

```text
Elastic Agent / Beats
        ↓
    Collection

Logstash
        ↓
Processing

Elasticsearch
        ↓
Storage + Search

Kibana
        ↓
Visualization + Investigation
```

---

## Logstash Pipeline

```text
Input

↓

Filter

↓

Output
```

---

## Complete SIEM Flow

```text
Log Source

↓

Collection

↓

Processing

↓

Normalization

↓

Elasticsearch

↓

Detection

↓

Alert

↓

Kibana

↓

SOC Analyst
```

---

## Example

```text
Windows Event 4625

↓

Winlogbeat / Elastic Agent

↓

Logstash

↓

Elasticsearch

↓

Detection Rule

↓

Brute Force Alert

↓

Kibana

↓

SOC Investigation
```

---

## Important Concepts

```text
Elastic Agent / Beats

→ Collect logs
```

```text
Logstash

→ Process and transform logs
```

```text
Elasticsearch

→ Store, index, and search logs
```

```text
Kibana

→ Search, visualize, and investigate
```

---

## Biggest Concept

```text
ELK-based SIEM

↓

Collect Security Logs

↓

Process & Normalize

↓

Store & Index

↓

Search & Correlate

↓

Detect Suspicious Activity

↓

Generate Alert

↓

Investigate

↓

Respond
```

The main purpose is to transform:

```text
Raw Logs
```

into:

```text
Structured Security Data

↓

Detection

↓

Actionable Security Information
```

---

*End of Working of ELK Stack for SIEM Notes*
