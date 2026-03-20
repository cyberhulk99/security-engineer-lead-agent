# Pentest Reporting Guide — Security Engineer Lead

## Report Structure

### Executive Summary (Non-Technical)
- Engagement objectives and scope
- Overall risk rating (Critical / High / Medium / Low)
- Top 3 most critical findings in plain language
- Business impact summary
- Key recommendations

### Technical Summary
- Methodology used (PTES, OWASP, NIST)
- Testing timeline
- Tools and techniques employed
- Scope covered vs. not tested

### Findings Table
| # | Title | Severity | CVSS | Status |
|---|-------|----------|------|--------|
| 1 | Remote Code Execution via SSTI | Critical | 9.8 | Open |
| 2 | SQL Injection in Login Form | Critical | 9.1 | Open |

### Individual Finding Template

```
=== FINDING [NUMBER] ===

Title:          [Action-oriented, descriptive]
Severity:       Critical / High / Medium / Low / Informational
CVSS v3.1:      [Score] - [Vector String]
CWE:            CWE-XXX — [Name]
CVE:            CVE-XXXX-XXXXX (if applicable)

DESCRIPTION
-----------
[2-3 sentences: what is the vulnerability, why does it exist,
 and where was it found specifically]

EVIDENCE
--------
Request:
  POST /api/login HTTP/1.1
  Host: target.com
  Content-Type: application/json

  {"username":"admin'--","password":"x"}

Response:
  HTTP/1.1 200 OK
  {"token":"eyJ...","user":"admin","role":"administrator"}

[Screenshots numbered: Figure 1.1, 1.2, etc.]

STEPS TO REPRODUCE
------------------
1. Navigate to https://target.com/login
2. Enter username: admin'--
3. Enter any password
4. Observe successful authentication

IMPACT
------
[Business impact - what can an attacker actually do?]
Example: "An unauthenticated attacker can bypass the login mechanism
and gain administrative access to all user accounts and sensitive data,
including PII for 50,000+ customers."

REMEDIATION
-----------
[Specific, actionable fix]
Short-term: [Immediate mitigation]
Long-term: [Proper fix]

Example:
  Short-term: Disable the affected endpoint until patched.
  Long-term: Implement parameterized queries / prepared statements.
  Use: $stmt = $pdo->prepare("SELECT * FROM users WHERE username = ?");

REFERENCES
----------
- OWASP: https://owasp.org/www-community/attacks/SQL_Injection
- CWE-89: https://cwe.mitre.org/data/definitions/89.html
- PortSwigger: https://portswigger.net/web-security/sql-injection
```

---

## CVSS v3.1 Quick Calculator

**Attack Vector (AV):** Network=0.85, Adjacent=0.62, Local=0.55, Physical=0.2
**Attack Complexity (AC):** Low=0.77, High=0.44
**Privileges Required (PR):** None=0.85, Low=0.62, High=0.27
**User Interaction (UI):** None=0.85, Required=0.62
**Scope (S):** Unchanged / Changed
**Confidentiality (C):** High=0.56, Low=0.22, None=0
**Integrity (I):** High=0.56, Low=0.22, None=0
**Availability (A):** High=0.56, Low=0.22, None=0

Use: https://www.first.org/cvss/calculator/3.1

---

## Severity Guidelines

| Rating | CVSS | Description |
|--------|------|-------------|
| Critical | 9.0–10.0 | Immediate exploitation, full system compromise |
| High | 7.0–8.9 | Significant impact, likely exploitable |
| Medium | 4.0–6.9 | Limited impact or requires interaction |
| Low | 0.1–3.9 | Minimal impact, defense in depth |
| Info | N/A | Observations, best practices, hardening |

---

## Attack Narrative Section

Include a prose section telling the story of the attack:

> "Beginning with passive reconnaissance, the team identified an exposed `.git` directory at
> `https://target.com/.git`, which revealed application source code including database credentials.
> Using these credentials, we accessed an internal PostgreSQL instance and extracted the users table
> containing 47,293 bcrypt-hashed passwords. Cross-referencing with the web application, we
> identified that the hash algorithm used insufficient work factor (cost=4), allowing offline
> cracking of 31% of passwords within 2 hours using a standard GPU cluster..."

---

## Appendices

### A — Scope
- In-scope IP ranges, domains, applications
- Out-of-scope items
- Testing window

### B — Methodology
- Standards followed
- Testing approach

### C — Tool List
- All tools used with versions

### D — Raw Evidence
- Full request/response logs
- Tool output logs
- Screenshots (labeled)
