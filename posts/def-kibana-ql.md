# Kibana Query Language (KQL)

> **Kibana Query Language (KQL)** is a query language used in Kibana to search, filter, and investigate data stored in Elasticsearch.
>
> KQL is commonly used in security monitoring and SIEM environments to quickly filter events based on fields, values, ranges, and logical conditions.

These notes cover:

* What is KQL
* Basic KQL Syntax
* Field and Value Searching
* Exact Value Searching
* Boolean Operators
* Grouping Conditions
* Wildcards
* Range Queries
* Existence Queries
* Text Searching
* Nested Fields
* KQL in Defensive Security
* Quick Revision Sheet

---

# Table of Contents

* [1. What is KQL](#1-what-is-kql)
* [2. Basic KQL Syntax](#2-basic-kql-syntax)
* [3. Field and Value Searching](#3-field-and-value-searching)
* [4. Exact Value Searching](#4-exact-value-searching)
* [5. Boolean Operators](#5-boolean-operators)
* [6. Grouping Conditions](#6-grouping-conditions)
* [7. Wildcards](#7-wildcards)
* [8. Range Queries](#8-range-queries)
* [9. Existence Queries](#9-existence-queries)
* [10. Text Searching](#10-text-searching)
* [11. Nested Fields](#11-nested-fields)
* [12. KQL in Defensive Security](#12-kql-in-defensive-security)
* [13. Quick Revision Sheet](#13-quick-revision-sheet)

---

# 1. What is KQL

KQL stands for:

```text
Kibana Query Language
```

It is used inside Kibana to:

```text
Search Data

Filter Data

Investigate Events
```

Basic flow:

```text
KQL Query

↓

Kibana

↓

Elasticsearch

↓

Matching Documents
```

KQL is mainly used for **filtering and searching** data.

---

# 2. Basic KQL Syntax

The basic syntax is:

```text
field:value
```

Example:

```text
event.code:4624
```

Meaning:

```text
Find documents where
event.code is 4624
```

---

# 3. Field and Value Searching

A KQL query normally consists of:

```text
Field

+

Value
```

Example:

```text
user.name:admin
```

Here:

```text
user.name
    ↓
Field

admin
    ↓
Value
```

---

## Common Security Fields

Examples:

```text
event.code

user.name

host.name

source.ip

destination.ip

process.name

process.command_line

file.name

dns.question.name
```

---

# 4. Exact Value Searching

A value can be searched directly:

```text
user.name:administrator
```

For a phrase containing spaces:

```text
process.command_line:"whoami /all"
```

---

## Keyword Fields

Some Elasticsearch fields have a `.keyword` version.

Example:

```text
user.name.keyword:administrator
```

Keyword fields are generally used when an exact value is required.

---

# 5. Boolean Operators

KQL supports logical operators:

```text
AND

OR

NOT
```

---

## AND

```text
event.code:4625 AND user.name:admin
```

Both conditions must match.

```text
Condition 1
    +
Condition 2
    ↓
Match
```

---

## OR

```text
event.code:4624 OR event.code:4625
```

Either condition can match.

```text
Condition 1
    OR
Condition 2
```

---

## NOT

```text
NOT event.code:4624
```

Excludes matching documents.

---

# 6. Grouping Conditions

Parentheses are used to group conditions.

Example:

```text
(event.code:4624 OR event.code:4625)
AND user.name:admin
```

The query is evaluated as:

```text
4624 OR 4625

        +

user.name = admin
```

Grouping is useful when combining multiple conditions.

---

# 7. Wildcards

KQL supports wildcard characters.

Main wildcards:

```text
*
?
```

---

## Asterisk `*`

Represents zero or more characters.

Example:

```text
process.name:powershell*
```

Can match values beginning with:

```text
powershell
powershell.exe
powershell_ise.exe
```

---

## Question Mark `?`

Represents a single character.

Example:

```text
host.name:web-0?
```

Can match:

```text
web-01
web-02
web-03
```

---

# 8. Range Queries

Range operators can be used with numeric and date fields.

Common operators:

```text
>

<

>=

<=
```

---

## Greater Than

```text
network.bytes > 1000000
```

---

## Less Than

```text
network.bytes < 1000000
```

---

## Greater Than or Equal

```text
network.bytes >= 1000000
```

---

## Less Than or Equal

```text
network.bytes <= 1000000
```

---

## Range Combination

```text
network.bytes >= 1000000
AND
network.bytes <= 5000000
```

---

# 9. Existence Queries

KQL can check whether a field exists.

Syntax:

```text
field:*
```

Example:

```text
source.ip:*
```

Meaning:

```text
source.ip exists
```

---

## Excluding Existing Fields

```text
NOT source.ip:*
```

Meaning:

```text
source.ip does not exist
```

---

# 10. Text Searching

KQL can also search text.

Example:

```text
failed login
```

A specific field can be searched:

```text
message:"failed login"
```

This searches the `message` field for the specified phrase.

---

# 11. Nested Fields

Security data often contains structured fields.

For example:

```text
host.name
user.name
source.ip
destination.ip
process.name
```

KQL can directly reference these fields:

```text
source.ip:192.168.1.10
```

```text
process.name:powershell.exe
```

```text
user.name:administrator
```

The exact field names depend on the data mapping used in Elasticsearch.

---

# 12. KQL in Defensive Security

KQL is useful for filtering security telemetry such as:

```text
Windows Events

Sysmon Events

PowerShell Logs

Authentication Logs

Network Logs

DNS Logs

Endpoint Telemetry
```

---

## Basic Security Query

```text
event.code:4625
```

Can be used to filter:

```text
Failed Logon Events
```

---

## Multiple Conditions

```text
event.code:4625
AND
source.ip:10.10.10.20
```

Filters events using:

```text
Event ID

+

Source IP
```

---

## Process Filtering

```text
process.name:powershell.exe
```

---

## Network Filtering

```text
source.ip:192.168.1.10
```

---

## DNS Filtering

```text
dns.question.name:example.com
```

The important concept is:

```text
Raw Security Data

↓

KQL Filter

↓

Relevant Events

↓

Investigation
```

---

# 13. Quick Revision Sheet

## Basic Query

```text
field:value
```

---

## AND

```text
field:value AND field:value
```

---

## OR

```text
field:value OR field:value
```

---

## NOT

```text
NOT field:value
```

---

## Grouping

```text
(field:value OR field:value)
AND field:value
```

---

## Wildcard

```text
field:value*
```

---

## Single Character

```text
field:value?
```

---

## Range

```text
field > value

field < value

field >= value

field <= value
```

---

## Field Exists

```text
field:*
```

---

## Field Does Not Exist

```text
NOT field:*
```

---

## Important Concept

```text
KQL

↓

Search

↓

Filter

↓

Relevant Events

↓

Security Investigation
```

KQL is primarily a **search and filtering language**. Its effectiveness in defensive security depends on having useful, correctly mapped telemetry in Elasticsearch.

---

#  References

The following official Elastic documentation provides complete details about ECS fields, Beats, and Windows log mappings. These resources are useful when creating KQL queries, writing detection rules, building dashboards, or understanding the meaning of individual log fields.

| Documentation | Purpose |
|---------------|---------|
| https://www.elastic.co/docs/reference/ecs | Complete Elastic Common Schema (ECS) reference. Lists all standardized field names and their descriptions. |
| https://www.elastic.co/docs/reference/ecs/ecs-event | Explains the **event.\*** fields such as `event.code`, `event.action`, `event.category`, `event.outcome`, and `event.kind`. Useful for event analysis and detection engineering. |
| https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-winlog | Lists Windows-specific fields collected by Winlogbeat, including `winlog.event_data.*`, `winlog.computer_name`, `winlog.channel`, and more. |
| https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-ecs | Shows ECS fields automatically populated by Winlogbeat, making Windows logs compatible with the Elastic Common Schema. |
| https://www.elastic.co/docs/reference/beats/winlogbeat/exported-fields-security | Documents security-related fields exported by Winlogbeat for authentication, users, processes, and security events. |
| https://www.elastic.co/docs/reference/beats/filebeat/exported-fields | Complete list of fields exported by Filebeat modules for application and system log collection. |
| https://www.elastic.co/docs/reference/beats/filebeat/exported-fields-ecs | ECS fields generated by Filebeat, helping normalize logs from Linux, web servers, databases, and applications. |


---

*End of Kibana Query Language Notes*
