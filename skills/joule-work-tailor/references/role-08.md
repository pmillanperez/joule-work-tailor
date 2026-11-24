# Role 8 — Product Manager / Industry Expert / SME

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

### Questions

**Q1:** What is your content focus?

1. Product strategy, roadmaps, and market positioning
2. Industry thought leadership and customer-facing content
3. Technical standards, compliance, and product documentation
4. AI product strategy or AI use case development
5. Business Transformation Management (LeanIX, Signavio, WalkMe)
6. Multiple of the above

*Type one number.*

**Q2:** Do you create content for external audiences (customers, analysts, marketing)?

1. Yes
2. No

*Type one number.*

Activity flags: FOCUS (from Q1), EXTERNAL(Q2=1)
*Always assumed: research=yes, documentation=yes. Skipped: coding, demos, value analysis.*

### Skill Queries
Base: `knowledge management`, `product requirements`, `writing SAP`, `documentation synthesis`, `research`, `branded presentation`, `writing style`
EXTERNAL (Q2=1): + `branded presentation`, `analyst briefing`, `content strategy`
FOCUS includes AI (Q1=4): + `AI use case`, `generative AI`, `AI governance`
FOCUS includes BTM (Q1=5): + `LeanIX`, `process intelligence`, `Signavio`, `digital adoption`
FOCUS includes standards (Q1=3): + `product standard`, `compliance documentation`

### MCP Queries
`knowledge graph`, `product standard`, `knowledge SAP`, `experience`

### Personalization Defaults
- Balance strategic framing with concrete detail; avoid vague generalities
- Apply SAP writing style guidelines for product content
- Cite SAP official sources; label analyst benchmarks clearly

### Agent & GitHub Catalog
**OneAI Spaces (Essential):** Product Knowledge BTM (if FOCUS=5)
**OneAI Spaces (Recommended):** Product Knowledge ERP, Product Knowledge Business Suite
**GitHub Skills:** Joule Studio 2.0 KB — github.tools.sap/SAP-Build-Enablement/Joule-Studio-and-SAP-Build-KB (INTERNAL, if FOCUS=4)
**AI-Generatable Templates:** None by default
**Personal Agent Template:** Role 1 base (substitute role name)

---
