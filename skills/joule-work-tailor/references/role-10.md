# Role 10 — Business Development / Field Sales

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

### Questions

**Q1:** Which of these describe your work?

1. I manage accounts or territory (pipeline, forecast, account planning)
2. I need competitive intelligence tools
3. I present to C-level or executive stakeholders
4. All of the above

*Type one number.*

Activity flags: ACCOUNT_MGMT(Q1=1or4), COMPETITIVE(Q1=2or4), PRESENTATIONS(Q1=3or4)
*Always assumed: presentations=yes, company research=yes. Skipped: coding, tech docs, demos.*

### Skill Queries
Base: `selling coach`, `account intelligence`, `CRM link`, `branded presentation`, `customer engagement`, `pipeline`
ACCOUNT_MGMT: + `account planning`, `territory analysis`
COMPETITIVE: + `competitive intelligence`, `battle card`
PRESENTATIONS: + `executive presentation`, `C-level briefing`

### MCP Queries
`customer strategy`, `experience`, `enterprise`, `knowledge SAP`

### Personalization Defaults
- Lead with business outcomes; avoid deep technical detail unless asked
- For executive communications, keep responses concise and action-oriented
- Use commercial names (RISE with SAP, SAP GROW) rather than technical product names

### Agent & GitHub Catalog
**OneAI Spaces (Essential):** Ask ONE360, Melody Help, [Ask Services] Cloud ERP
**OneAI Spaces (Recommended):** APM Pricing Agent, SAP HCM Commercial Desk
**GitHub Skills:** None specific
**AI-Generatable Templates:** T2 (Presales Strategy Partner if PRESENTATIONS or ACCOUNT_MGMT) — see agents.md Section 3 for full T2 role eligibility
**Personal Agent Template:** Role 1 base (substitute role name)

---
