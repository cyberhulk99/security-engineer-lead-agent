# 🔴 Security Engineer Lead Agent

> **Autonomous Senior Security Engineer Lead for Claude Code**
> Full-spectrum offensive security agent — Web, Network, API, Cloud, Crypto, Business Logic.

---

## What Is This?

A **Claude Code agent** that thinks and acts as a Lead Security Engineer with 15+ years of
offensive security experience. Drop it into any project and get a fully autonomous red team
agent that can plan, execute, and report on complete penetration test engagements.

```
THINK → PLAN → EXECUTE → ADAPT → REPORT
```

---

## Capabilities

| Domain | Coverage |
|--------|----------|
| 🌐 Web Application | OWASP Top 10, Business Logic, Auth Bypass, IDOR, SSTI, XXE, Deserialization |
| 🔌 Network PT | Active Directory, Kerberoasting, Pass-the-Hash, Pivoting, Lateral Movement |
| ☁️ Cloud | AWS/GCP/Azure IAM abuse, SSRF→Metadata, S3 exposure, Privilege Escalation |
| 🔐 Cryptography | JWT attacks, TLS/SSL audit, Padding Oracle, Hash cracking, ECB manipulation |
| 🔗 API Security | BOLA/IDOR, Mass Assignment, GraphQL introspection, Rate limit bypass |
| 🧩 Business Logic | Price tampering, Workflow bypass, Race conditions, Account takeover chains |
| 👻 Evasion | WAF bypass, AV/EDR evasion, C2 blending, Anti-forensics |

---

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
- Anthropic API key configured
- Target authorization (pentest agreement / bug bounty scope / lab environment)

### Install Claude Code
```bash
npm install -g @anthropic-ai/claude-code
```

---

## Installation

### Option 1 — Clone into existing project
```bash
cd your-project/
git clone https://github.com/YOUR_USERNAME/security-engineer-lead-agent .claude-sec
cp -r .claude-sec/.claude ./
cp .claude-sec/CLAUDE.md ./
```

### Option 2 — Clone as standalone workspace
```bash
git clone https://github.com/YOUR_USERNAME/security-engineer-lead-agent
cd security-engineer-lead-agent
claude
```

---

## Usage

### Start Claude Code
```bash
claude
```

### Invoke the Agent
```
Use the security-engineer-lead agent to pentest https://target.com
```

Or directly:
```
/agents
```
Select `security-engineer-lead` from the list.

### Example Prompts

```
Perform a full web application pentest on https://target.com
Authorization: Bug bounty program — scope: *.target.com
```

```
Run an Active Directory attack chain against 192.168.1.0/24
Authorization: Internal pentest — signed agreement on file
```

```
Perform JWT security assessment on this token: eyJ...
Target: https://api.target.com (authorized lab environment)
```

```
Cloud security assessment for our AWS account
Authorization: Internal security review — full account access
```

---

## Repository Structure

```
security-engineer-lead-agent/
│
├── CLAUDE.md                                    # Agent thinking persona & inner monologue
│
└── .claude/
    ├── agents/
    │   └── security-engineer-lead.md           # Main agent definition
    │
    └── skills/
        └── security-engineer-lead/
            ├── SKILL.md                         # Skill routing & overview
            └── references/
                ├── vulnerability-domains.md     # Attack playbooks per domain
                ├── exploitation-playbooks.md    # Step-by-step exploit chains
                ├── evasion-techniques.md        # WAF/AV/EDR bypass
                └── reporting-guide.md           # Pentest report templates
```

---

## How the Agent Thinks

### The Inner Monologue (from `CLAUDE.md`)

| Step | Question |
|------|----------|
| 🔍 THINK | What's the crown jewel? What would a real attacker want? |
| 🧩 PLAN | What assumptions can I break? What's my attack plan? |
| ⚡ EXECUTE | What's my highest-probability vector? Start there. |
| 🔄 ADAPT | Blocked? What's the alternate path? |
| 📋 REPORT | What's the business impact? How do I fix it? |

### Every Finding Includes
- **Title** — action-oriented, descriptive
- **Severity** — Critical / High / Medium / Low / Info
- **CVSS v3.1** — score + vector string
- **Evidence** — exact request/response or PoC
- **Business Impact** — not just technical impact
- **Remediation** — short-term + long-term fix
- **References** — CVE, CWE, OWASP

---

## ⚖️ Legal & Ethics

**This tool is for authorized security testing only.**

- Always obtain written authorization before testing any target
- Never use against systems you do not own or have explicit permission to test
- Unauthorized access to computer systems is illegal in most jurisdictions
- The author is not responsible for misuse of this tool

Supported use cases:
- ✅ Penetration tests with signed agreement
- ✅ Bug bounty programs (within stated scope)
- ✅ CTF competitions and lab environments (HackTheBox, TryHackMe, DVWA, etc.)
- ✅ Internal security assessments on systems you own
- ❌ Unauthorized testing of any system

---

## Toolchain

The agent reasons about and can guide usage of:

```
Web:      Burp Suite Pro, Nuclei, SQLMap, dalfox, ffuf, feroxbuster, jwt_tool, SSRFmap
Network:  nmap, masscan, Metasploit, Impacket, CrackMapExec, BloodHound, Rubeus
Cloud:    Pacu, ScoutSuite, Prowler, CloudMapper, AWS/gcloud/az CLI
Crypto:   testssl.sh, sslscan, hashcat, john, PadBuster, jwt_tool
OSINT:    amass, subfinder, dnsx, httpx, theHarvester, shodan, gau, katana
Post:     Chisel, proxychains, pwntools, Cobalt Strike concepts, Sliver
```

---

## Contributing

Pull requests welcome. Areas to contribute:
- New attack playbooks in `references/vulnerability-domains.md`
- New exploit chains in `references/exploitation-playbooks.md`
- New evasion techniques in `references/evasion-techniques.md`
- Additional agent personas (e.g., `defensive-security-lead`, `cloud-security-lead`)

---

## License

MIT License — see `LICENSE` file.

---

> *"I am the Lead. I own this engagement. I think before I act, I document as I go,
> I chain everything I find, and I deliver findings that make the client's security
> materially better."*
