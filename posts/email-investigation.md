# Email Attribution Investigation Write-up

> This write-up demonstrates an OSINT-based email attribution investigation using metadata correlation, contextual reduction, and open-source enrichment techniques.

**Category:** OSINT / Email Attribution / Investigation  
**Investigation Type:** Email-Centric OSINT Investigation
---

# Table of Contents

1. [Initial Case Information](#1-initial-case-information)
2. [Available Evidence](#2-available-evidence)
3. [Investigation Methodology](#3-investigation-methodology)
4. [Phase 1 — Initial Enumeration](#4-phase-1--initial-enumeration)
5. [Phase 2 — Recovery Email Analysis](#5-phase-2--recovery-email-analysis)
6. [Phase 3 — Context Reduction](#6-phase-3--context-reduction)
7. [Phase 4 — Open Source Enrichment](#7-phase-4--open-source-enrichment)
8. [Phase 5 — Relationship Analysis](#8-phase-5--relationship-analysis)
9. [Phase 6 — Recovery Email Validation](#9-phase-6--recovery-email-validation)
10. [Phase 7 — Secondary Pivot Analysis](#10-phase-7--secondary-pivot-analysis)
11. [Evidence Assessment](#11-evidence-assessment)
12. [Final Assessment](#12-final-assessment)
13. [Key Investigative Lesson](#13-key-investigative-lesson)

---

# 1. Initial Case Information

Victim:

```text
Susmita Pal
```

Potential Account Owner:

```text
Archana Ghosh
```

Initial artifact:

```text
dkm.cse.research01@gmail.com
```

Available context:

```text
PhD admission related communication
```

Primary challenge:

```text
Reduce uncertainty using one email artifact
```

---

# 2. Available Evidence

Initial evidence available:

### Evidence E-01

Email account:

```text
dkm.cse.research01@gmail.com
```

### Evidence E-02

```text
Email body indicating that the email was sent from Dev Kumar Maity.
```

### Evidence E-03

```text
Institutional admission context
```

No direct identifiers were initially available. An additional objective was to determine whether Dev Kumar Maity was the actual owner of the account. If confirmed, further details would be collected; otherwise, the investigation would focus on identifying the individual operating the account.

---

# 3. Investigation Methodology

Investigation workflow:

```text
Artifact Collection
        ↓
Metadata Enumeration
        ↓
Candidate Identification
        ↓
Open Source Enrichment
        ↓
Correlation
        ↓
Validation
        ↓
Assessment
```

---

# 4. Phase 1 — Initial Enumeration

The investigation began with metadata collection associated with:

```text
arc.cse.research01@gmail.com
```

Recovered information:

Google Account Identifier:

```text
118181039247757946740
```

Observed last update:

```text
2025-02-07 05:23:55 UTC
```

This information alone was insufficient for attribution.

However, account recovery analysis produced a useful pivot.

---

# 5. Phase 2 — Recovery Email Analysis

During account recovery workflow analysis a partial recovery email hint was observed:

```text
arc********@gmail.com
```

This became the first significant clue. The observed recovery hint did not strongly align with the name Dev Kumar Maity because the visible pattern suggested additional characters not reflected in the available identity.

Investigation focus shifted:

```text
Unknown account owner
            ↓
Identity beginning with:
arc
```

---

# 6. Phase 3 — Context Reduction

Rather than searching globally, the investigation reduced the search space using contextual evidence.

Relevant document examined:

```text
weblist_CSE_PhD.pdf
```

Objective:

Identify names containing:

```text
arc
```

Result:

One matching candidate identified:

```text
Archana Ghosh
```

Reference:

```text
Application ID: 855
```

At this stage the individual was not considered a confirmed suspect.

Designation:

```text
Potential Account Owner
```

---

# 7. Phase 4 — Open Source Enrichment

Open-source investigation collected additional public indicators regarding Archana Ghosh.

Observed findings:

- M.Tech graduate (2020)
- Academic affiliations identified
- Employment references discovered
- Public profile footprints identified

Recovered institutional email:

```text
archana.ghosh@asu.ac.in
```

Additional indicators:

- academic records
- public profile presence
- educational references

No direct attribution conclusions were made.

---

# 8. Phase 5 — Relationship Analysis

Primary question:

```text
Is there a relationship between
Susmita Pal and Archana Ghosh?
```

Observed indicators:

- similar academic environment
- institutional proximity
- profile recommendations

No direct evidence found showing:

- communication
- interaction
- project collaboration
- direct association

Assessment:

```text
Evidence Strength:
LOW
```

---

# 9. Phase 6 — Recovery Email Validation

New hypothesis:

Could the recovery email belong to Archana Ghosh?

Observed recovery pattern:

```text
arc********@gmail.com
```

Naming variations tested:

```text
archanaghosh0@gmail.com
archanaghosh1@gmail.com
...
archanaghosh9@gmail.com
```

Observed active address:

```text
archanaghosh9@gmail.com
```

Further validation suggested alignment with:

```text
dkm.cse.research01@gmail.com
```

The email address archanaghosh9@gmail.com was tested during recovery workflow validation and appeared to match the recovery information associated with the target account. The validation process suggested that archanaghosh9@gmail.com was associated as a recovery email for dkm.cse.research01@gmail.com. 

Recovered Google account identifier:

```text
110557315227024853086
```

Observed update time:

```text
2025-02-07 05:23:56 UTC
```

Interesting observation:

```text
Timestamp difference:
A timestamp difference of approximately 1 second was observed between the associated account metadata.
```

```text
Recovery Email Identified:
Recovery email address of dkm.cse.research01@gmail.com is archanaghosh9@gmail.com .
```
This represented the strongest correlation discovered.

---

# 10. Phase 7 — Secondary Pivot Analysis

Additional public indicators discovered:

Email accounts:

```text
archana.asu@gmail.com

archanaghosh25@gmail.com

archanaghosh9@gmail.com
```

Observed nickname:

```text
Anu
```

Observed device:

```text
Galaxy A12
```

Purpose:

```text
Enrichment only
```

These indicators alone do not establish attribution. Additional information such as phone numbers, family references, and address details was identified during enrichment. These details are intentionally omitted from this public write-up. All names and identifiers used in this write-up have been modified to preserve privacy.

---

# 11. Evidence Assessment

| Indicator | Confidence |
|------------|------------|
| Recovery hint starts with arc | High |
| Candidate identified from context | Medium |
| Public profile proximity | Low |
| Recovery email validation | High |
| Account linkage | Medium–High |
| Final attribution | Moderate |

---


# 12. Final Assessment

Investigation findings:

✓ Search space reduction

✓ Metadata correlation

✓ Contextual analysis

✓ Candidate identification

✓ Recovery workflow validation

Assessment:

```text
dkm.cse.research01@gmail.com
            ↕
archanaghosh9@gmail.com

```

Important distinction:

```text
Account linkage identified

```

Current conclusion:

```text
Potential Account Association Identified
```

---

# 13. Key Investigative Lesson

Avoid:

```text
One clue → One suspect
```

Prefer:

```text
Artifact
    ↓
Pivot
    ↓
Correlate
    ↓
Validate
    ↓
Assess Confidence
```

The objective is evidence-driven attribution.

---

*End of Investigation Write-up*