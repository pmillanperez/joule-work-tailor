# Role 4 — Delivery / Implementation Consultant

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

### Questions

**Q1:** Which SAP solution(s) do you implement?

1. SAP S/4HANA (public or private cloud)
2. SAP SuccessFactors
3. SAP BTP and integrations
4. SAP Ariba / Concur / Fieldglass
5. Multiple / All

*Type numbers separated by commas, or 'all'.*

**Q2:** Which of these describe your work?

1. Integration specs, technical architecture, or configuration docs
2. Project planning, go-live readiness, or cutover activities
3. Both
4. Neither

*Type one number.*

Activity flags: SOLUTION, TECH_DOCS (always assumed), PROJECT_MGMT(Q2=2or3)
*Always assumed: technical documentation=yes. Skipped: presentations, demos, value analysis.*

### Skill Queries
Base: `project delivery`, `technical writing`, `go-live SAP`, `SAP documentation`, `writing copilot`, `implementation guide`, `meeting notes`, `upgrade migration`
SOLUTION includes S/4HANA: + `S4HANA`, `ABAP`
SOLUTION includes BTP: + `BTP deploy`, `integration suite`
SOLUTION includes SuccessFactors: + `SuccessFactors`, `HR implementation`
PROJECT_MGMT: + `project checklist`, `cutover planning`

### MCP Queries
`jira`, `knowledge SAP`, `github`

### Personalization Defaults
- Prefer numbered step-by-step formats for process guidance
- Include SAP Activate phase context where relevant
- Be precise with configuration paths and object names

### Agent & GitHub Catalog
**OneAI Spaces (Essential):** Product Knowledge ERP, BASE
**OneAI Spaces (Recommended):** Product Knowledge HCM (if SuccessFactors), Product Knowledge Spend (if Ariba)
**GitHub Skills:** None specific
**AI-Generatable Templates:** None (coding flag not typical for this role)
**Personal Agent Template:** Role 1 base (substitute role name)

---
