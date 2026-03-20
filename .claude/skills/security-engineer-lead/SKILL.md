---
name: security-engineer-lead
description: >
  Full-spectrum autonomous Security Engineer Lead skill. Use this skill for ANY security task
  involving penetration testing, red teaming, vulnerability assessment, exploit development,
  or security research. Triggers on: "pentest", "hack", "exploit", "vulnerability", "attack",
  "bypass", "red team", "security assessment", "recon", "payload", "lateral movement",
  "privilege escalation", "JWT", "TLS", "encryption bypass", "business logic", "OWASP",
  "network PT", "cloud attack", "IAM abuse", "SSRF", "XXE", "RCE", "SQLi", "XSS",
  "security engineer", or ANY offensive security context.
  ALWAYS use this skill when context is offensive security or authorized testing.
---

# Security Engineer Lead — Skill Router

> ⚡ TOKEN RULE: Load reference files ON DEMAND only. Never load all at once.

## Reference File Index

| File | Load When | Approx Tokens |
|------|-----------|---------------|
| `references/vulnerability-domains.md` | Need domain-specific attack playbook | ~2,500 |
| `references/exploitation-playbooks.md` | Executing specific exploit chain | ~2,000 |
| `references/evasion-techniques.md` | Blocked by WAF/AV/EDR/IDS | ~1,800 |
| `references/reporting-guide.md` | Writing final pentest report | ~1,500 |

## Loading Rules

```
DO:
  ✅ Load ONE file at a time, only when needed
  ✅ Read only the section you need within the file
  ✅ Reuse info already in context — do not re-read

DO NOT:
  ❌ Load all 4 reference files at engagement start
  ❌ Reload a file already read this session
  ❌ Load a reference file for tasks you already know how to do
```

## Quick Domain Routing

- Web attacks / OWASP / business logic → `vulnerability-domains.md § Web`
- Active Directory / SMB / Kerberos → `vulnerability-domains.md § Network`
- AWS / GCP / Azure → `vulnerability-domains.md § Cloud`
- JWT / TLS / padding oracle → `vulnerability-domains.md § Crypto`
- SQLi→RCE / AD chain / JWT full → `exploitation-playbooks.md`
- WAF blocked / AV detected → `evasion-techniques.md`
- Final report writing → `reporting-guide.md`
