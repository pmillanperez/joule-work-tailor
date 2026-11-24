# Role 3 — Value Advisor / Value Engineer

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

### Questions

**Q1:** What is your primary focus?

1. Financial business cases and ROI analysis
2. Value maps and industry benchmarks
3. Deal structuring and commercial positioning
4. All of the above

*Type one number.*

**Q2:** Which of these describe your work?

1. I research customer industries or peer benchmarks
2. I create customer-facing presentations or executive briefs
3. Both
4. Neither

*Type one number.*

Activity flags: FOCUS, RESEARCH(Q2=1or3), PRESENTATIONS(Q2=2or3)
*Always assumed: value analysis=yes. Skipped: coding, demos, deep tech docs.*

### Skill Queries
Base: `value map`, `business case`, `selling coach`, `account intelligence`, `industry benchmark`, `executive briefing`, `branded presentation`
RESEARCH: + `company research`, `customer intelligence`, `annual report`
PRESENTATIONS: + `corporate pptx`, `executive briefing`
FOCUS includes deal structuring (Q1=3): + `pricing deal`, `TCO ROI`

### MCP Queries
`customer strategy`, `experience`, `knowledge SAP`

### Personalization Defaults
- Prioritize quantified business outcomes over technical detail
- Label estimates vs. confirmed data clearly
- Default to executive-level language; expand only when asked

### Agent & GitHub Catalog
**OneAI Spaces (Essential):** Ask ONE360, [Ask Services] Cloud ERP, APM Pricing Agent
**OneAI Spaces (Recommended):** [Ask Services] Value Acceleration, Melody Help
**GitHub Skills:** None specific
**AI-Generatable Templates:** T2 (Presales Strategy Partner if PRESENTATIONS or RESEARCH) — see agents.md Section 3 for full T2 role eligibility
**Personal Agent Template:** Role 1 base (substitute role name)

---
