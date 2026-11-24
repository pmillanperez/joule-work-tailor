# Role 5 — Data and AI Specialist / Analytics Architect

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

## Shared Tables Referenced

### TABLE C — Data and AI Products (drill-down from TABLE B option 1 or 3)

1. SAP HANA Cloud
2. SAP Datasphere
3. SAP Analytics Cloud (SAC)
4. SAP Business Data Cloud (BDC)
5. SAP Knowledge Graph
6. All of the above

---

### Questions

**Q1:** Which SAP Data and AI products do you work with? (TABLE C — multi-select)
This drives all subsequent queries — always ask first.

**Q2:** Which of these describe your work?

1. I build demos or proofs of concept for customers
2. I develop or code (BTP, Python, SQL, CAP)
3. Both
4. Neither — I primarily configure and analyse

*Type one number.*

**Q3 — Demo Environments** (only ask if Q2 includes 1 or 3):

Which demo environment will you use for Data and AI demos?

1. C39 — Public Cloud EMEA Standard (SAP Datasphere in BDC + full S/4HANA suite)
2. GLA-26Q2 — Private Cloud Long-Term (SAP Datasphere in BDC + IBP + full suite)
3. Both C39 and GLA-26Q2
4. I build custom environments (Datasphere space / SAP HANA Cloud workspace)
5. Not sure yet

*Type one number.*

After selection, look up the dsp (Datasphere) endpoint for each chosen landscape in `references/demo-environments.md`. Store as DEMO_ENVS.
If Q3=4, add sap-bdc-advisor and data-product-mentor to Essential skills.

Activity flags: PRODUCTS (from Q1), DEMOS(Q2=1or3), CODING(Q2=2or3), DEMO_ENVS(from Q3 if Q2=1or3)
*Always assumed: data & AI domain=yes. Skipped: value analysis, company research.*

### Skill Queries
Base: `data visualization`, `generative AI`, `analytics`, `AI architecture SAP`, `data analyzer`, `SAC optimization`
PRODUCTS includes HANA Cloud: + `HANA cloud`, `vector search`
PRODUCTS includes Datasphere: + `Datasphere`, `data product`
PRODUCTS includes BDC: + `business data cloud`, `BDC advisor`
PRODUCTS includes AI Core/GenAI: + `AI Core`, `LLM orchestration`, `prompt engineering`
PRODUCTS includes Knowledge Graph: + `knowledge graph`, `SAP Knowledge Graph`
DEMOS: + `demo guide`, `WCI demo`, `branded presentation`, `customer prep`
CODING: + `CAP cloud application`, `BTP deploy`, `Python data`

### MCP Queries
`knowledge graph`, `BTP`, `experience`, `data`

### Personalization Defaults
- Use official SAP product names from sap-portfolio.md (never 'DWC', 'HANA' alone)
- Distinguish SAP Business AI (application-level) from SAP AI Core (infrastructure) from AI Foundation (developer toolkit)
- Include code examples when relevant to data engineering or AI topics

### Agent & GitHub Catalog
**OneAI Spaces (Essential):** Golden Ticket to Data Products, Cloud Reporting
**OneAI Spaces (Recommended):** ACTION — Integration Suite use case, Product Knowledge ERP
**GitHub Skills:** Joule A2A Agent — github.com/The-Joule/sap-joule-ai-toolkit (if CODING); SAP Joule Capability — github.com/SAP/custom-agentic-cookbook (if CODING)
**AI-Generatable Templates:** T1 (SAP Product Expert, per PRODUCTS)
**Personal Agent Template:** Role 5

---
