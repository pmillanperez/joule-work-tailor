# Role 7 — Customer Success Manager / Adoption Specialist

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

1. I track customer health scores, adoption metrics, or CSAT/NPS
2. I manage renewal, expansion, or at-risk customer conversations
3. I prepare executive-level presentations or status reports
4. All of the above

*Type one number.*

Activity flags: HEALTH(Q1=1or4), RENEWALS(Q1=2or4), PRESENTATIONS(Q1=3or4)
*Always assumed: customer engagement=yes, company research=yes. Skipped: coding, deep tech docs, demos.*

### Skill Queries
Base: `customer engagement`, `onboarding SAP`, `customer briefing`, `customer status`, `meeting transcript`, `branded presentation`, `account intelligence`
HEALTH: + `customer health`, `adoption tracker`
RENEWALS: + `renewal expansion`, `at-risk customer`
PRESENTATIONS: + `branded presentation`, `executive briefing`

### MCP Queries
`enterprise`, `customer strategy`, `knowledge SAP`

### Personalization Defaults
- Lead with customer outcomes and business impact
- Keep SAP internal terminology out of customer-facing drafts
- Meeting summaries should use action-item format with owners and dates

### Agent & GitHub Catalog
**OneAI Spaces (Essential):** Ask ONE360, CSM Process and Operations, AIgor — Adoption Programs
**OneAI Spaces (Recommended):** [Ask Services] Advanced Success Plan, [Ask Services] Max Success Plan, ASP Chatbot SharePoint, Global SPM/TQM Community
**GitHub Skills:** None specific
**AI-Generatable Templates:** None (CSM work is highly customer-contextual; no general template provides sufficient value)
**Personal Agent Template:** Role 7

---
