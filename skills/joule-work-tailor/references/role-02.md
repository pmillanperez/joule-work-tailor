# Role 2 — Enterprise Architect / Solution Architect

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

## Shared Tables Referenced

### TABLE A — SAP Portfolio Areas

1. Cloud ERP (SAP S/4HANA, RISE with SAP, GROW with SAP)
2. Business AI Platform – BAIP (Data and AI + Technology Foundation)
3. Customer Experience (SAP Sales Cloud, Service Cloud, Commerce Cloud)
4. Supply Chain (SAP IBP, Digital Manufacturing, EWM, TM)
5. Finance & Spend (Finance/Treasury in S/4HANA, SAP Ariba, Concur, Fieldglass)
6. Human Capital Management (SAP SuccessFactors)
7. Business Transformation Management (SAP LeanIX, Signavio, WalkMe)
8. All / Cross-portfolio

### TABLE B — BAIP Sub-areas (drill-down from TABLE A option 2)

1. Data and AI (SAP Business Data Cloud, SAP HANA Cloud, SAP Datasphere, SAP Analytics Cloud, SAP Knowledge Graph)
2. Technology Foundation (SAP BTP, AI Foundation, SAP AI Core, Generative AI Hub, Joule Studio, SAP Integration Suite, SAP Build, ABAP Cloud)
3. Both / Full BAIP

### TABLE C — Data and AI Products (drill-down from TABLE B option 1 or 3)

1. SAP HANA Cloud
2. SAP Datasphere
3. SAP Analytics Cloud (SAC)
4. SAP Business Data Cloud (BDC)
5. SAP Knowledge Graph
6. All of the above

### TABLE D — Technology Foundation Services (drill-down from TABLE B option 2 or 3)

1. AI Foundation (SAP AI Core, Generative AI Hub, Joule Studio)
2. SAP Integration Suite (cloud integration, API management, event mesh)
3. SAP Build (Apps, Process Automation, Code, Work Zone)
4. SAP CAP / ABAP Cloud (development framework)
5. SAP Cloud ALM (application lifecycle management)
6. All of the above

### TABLE E — Developer Platforms

1. ABAP (on-premise or ABAP Cloud)
2. SAP BTP & CAP (Cloud Application Programming Model)
3. SAP Integration Suite (iFlows, adapters, API proxies)
4. SAP Build (Apps, Process Automation, Code)
5. Multiple / All

---

### Questions

**Q1:** Which technology area do you focus on? (TABLE A — multi-select; skip options 3, 5, 6)

1. Cloud ERP
2. Business AI Platform – BAIP
4. Supply Chain
7. Business Transformation Management
8. All

*Type numbers separated by commas.*
*(Re-number options 1–5 when presenting to the user. Map answers back to TABLE A keys: displayed 1=TABLE A 1, 2=2, 3=4, 4=7, 5=8.)*

- If AREAS includes 2 (BAIP) or 8 (All): ask **Q1-BAIP** (TABLE B — single select)
  - If TABLE B = 1 or 3: ask **Q1a** (TABLE C — multi-select)
  - If TABLE B = 2 or 3: ask **Q1b** (TABLE D — multi-select)

**Q2:** Which of these describe your work?

1. I create customer-facing architecture presentations or deliverables
2. I develop or prototype solutions myself (code)
3. Neither

*Type one number.*

If Q2=2: **Q2a:** Which platform? (TABLE E — single select)

Activity flags: AREAS, BAIP_SUBAREAS, PRESENTATIONS(Q2=1), CODING(Q2=2), CODING_PLATFORM
*Always assumed: technical documentation=yes.*

### Skill Queries
Base: `architecture diagram`, `drawio`, `BTP`, `arc42`, `architecture decision`, `integration design`, `technical documentation`, `LeanIX`
PRESENTATIONS: + `branded presentation`, `SAP slides`
CODING ABAP: + `ABAP clean core`, `ABAP unit test`
CODING BTP/CAP: + `CAP cloud application`, `BTP deploy`
CODING Integration: + `CPI iFlow`, `integration suite`
AREAS includes BAIP Data and AI: + `data visualization`, `AI architecture SAP`
AREAS includes 7 (BTM): + `LeanIX enterprise`, `process mining`, `Signavio`

### MCP Queries
`jira`, `github`, `knowledge graph`, `knowledge SAP`

### Personalization Defaults
- Structure technical answers with clear headings and trade-off sections
- Reference official SAP architecture guidelines where applicable
- Include constraints and dependencies in recommendations

### Agent & GitHub Catalog
**OneAI Spaces (Essential):** Product Knowledge BTM, Product Knowledge ERP, Product Knowledge Business Suite
**OneAI Spaces (Recommended):** ACTION — Integration Suite use case (if BAIP Tech Foundation)
**GitHub Skills:** SAP Joule Capability — github.com/SAP/custom-agentic-cookbook (EXTERNAL only)
**AI-Generatable Templates:** T3 (Application Design Coach if PRESENTATIONS flag)
**Personal Agent Template:** Role 1 base (substitute role name)

---
