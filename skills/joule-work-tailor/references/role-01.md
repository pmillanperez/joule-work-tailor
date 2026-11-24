# Role 1 — Solution Advisor / Solution Engineer

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

---

### Questions

**Q1:** Which SAP areas do you cover? (TABLE A — multi-select)
- If AREAS includes 2 (BAIP) or 8 (All / Cross-portfolio): ask **Q1-BAIP**: Which BAIP sub-area? (TABLE B — select one or more)
  - If TABLE B = 1 or 3 (Data and AI): ask **Q1a**: Which Data and AI products? (TABLE C — multi-select)
  - If TABLE B = 2 or 3 (Technology Foundation): ask **Q1b**: Which Technology Foundation services? (TABLE D — multi-select)

**Q2:** Which of these describe your work?

1. I create or customize presentations and proposals for customers
2. I run live demos connected to SAP demo systems
3. I write technical documents (RFPs, architecture specs, proposals)
4. I do value analysis, ROI calculations, or business cases
5. I research companies or build customer profiles before meetings
6. I adapt or repurpose existing SAP content for specific customers

*Type numbers separated by commas, or 'all'.*

Activity flags: PRESENTATIONS(Q2=1), DEMOS(Q2=2), TECH_DOCS(Q2=3), VALUE(Q2=4), RESEARCH(Q2=5), CONTENT_ADAPT(Q2=6)

**Q3 — Demo Environments** (only ask if Q2 includes 2):
Read `references/demo-environments.md` Role-Based Landscape Selection Guide. Build the numbered list using ONLY landscapes that intersect with the user's AREAS. Always include the final two options.

**If ALL selected AREAS are exclusively 7 (BTM): skip the landscape picker entirely.** Instead inform: "BTM solutions (SAP LeanIX, Signavio, WalkMe) use dedicated cloud tenants, not SIX landscapes. Contact your BTM demo team for demo access." Do not present the landscape list.

LANDSCAPE FILTER — show option when:
```
C39 — S/4HC EMEA Standard (Public Cloud)        → AREAS includes 1, 3, 4, 5 (Finance&Spend), or BAIP+Data&AI (2a)
GLA-26Q2 — Long-Term Private Cloud (Full Suite)  → AREAS includes 1, or BAIP+Data&AI (2a), or 3, 4, or 5
GSA — Weekly Refreshed Private Cloud            → AREAS includes 1, 3, or 4
NQO — S/4HC US Standard (Public Cloud)          → AREAS includes 1 (US-region customers)
D2P — Public Cloud Custom II                    → AREAS includes 5 (Finance&Spend) or 6 (HCM)
JI4 — EMEA Custom (CX, Commerce, Subscriptions) → AREAS includes 3 (CX)
IBP dedicated tenants (NA / EMEA / ANZ)         → AREAS includes 4 (Supply Chain)
CWF — Financial Services                        → AREAS includes 1 (financial sector customers)
P1A / I4M — Public Sector                       → AREAS includes 1 (public sector customers)
[note] BTM (area 7): LeanIX, Signavio, WalkMe have their own cloud tenants — no SIX landscape
[always] Multiple / I'll choose per customer
[always] Not sure yet — guide me
```

Re-number filtered set from 1. After selection: look up SERVER TABLE per landscape in `references/demo-environments.md`. Keep only servers matching AREAS. Always include the core S/4HANA server. Store as DEMO_ENVS = {landscape, services[]}.

Full activity flags: AREAS, BAIP_SUBAREAS(from Q1-BAIP), PRESENTATIONS, DEMOS, TECH_DOCS, VALUE, RESEARCH, CONTENT_ADAPT, DEMO_ENVS

### Skill Queries
Base: `selling coach`, `customer meeting`, `customer prep`, `presales`, `account intelligence`, `SAP Minutes`
AREAS includes 1 (ERP): + `S4HANA cloud ERP`
BAIP includes Data and AI: + `data visualization`, `generative AI`, `analytics SAC`, `BDC advisor`, `data product`, `knowledge graph`
BAIP includes Technology Foundation: + `BTP architecture`, `integration SAP`, `AI Core`, `Joule Studio`
AREAS includes 3 (CX): + `customer experience CRM`
AREAS includes 4 (Supply Chain): + `supply chain IBP`
AREAS includes 5 (Finance & Spend): + `finance treasury`, `spend management Ariba`
AREAS includes 7 (BTM): + `LeanIX`, `process intelligence`, `Signavio`, `WalkMe`, `digital adoption`
PRESENTATIONS: + `branded presentation`, `corporate pptx`, `SAP slides`
DEMOS: + `demo guide`, `WCI demo`, `demo system`
VALUE: + `business case ROI`, `industry benchmark`
TECH_DOCS: + `technical writing`, `RFP proposal`
RESEARCH: + `company research`, `customer intelligence`
CONTENT_ADAPT: + `adapt use case`, `content personalization`, `customer proof`

### MCP Queries (general connectors only — demo connectors come from demo-environments.md)
`experience`, `customer strategy`, `knowledge`, `ADAPT`

### Personalization Defaults
- Use official SAP product names at all times
- Professional tone for C-level and IT decision-maker audiences
- Avoid SAP internal jargon in customer-facing content
- When DEMOS flag is set, include connected landscape names in the generated Role Description block

### Agent & GitHub Catalog
**OneAI Spaces (Essential):** Ask ONE360, Product Knowledge (per AREAS), Melody Help, [Ask Services] Cloud ERP
**OneAI Spaces (Recommended):** ACTION — Meeting Minutes, [Ask Services] Value Acceleration
**GitHub Skills:** SAP Joule Module (Community) — github.com/vigneshbarani24/sap-superpowers (EXTERNAL only)
**AI-Generatable Templates:** T1 (SAP Product Expert, per AREAS), T2 (Presales Strategy Partner if PRESENTATIONS/DEMOS/RESEARCH), T4 (Demo Prep Assistant if DEMOS + DEMO_ENVS set)
**Personal Agent Template:** Role 1

---
