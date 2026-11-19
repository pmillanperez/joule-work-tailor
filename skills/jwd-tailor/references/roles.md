# Role Configuration — Structured Questions, Queries, Personalization Defaults

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

## SHARED OPTION TABLES

Reuse these tables across roles. Present them using standard markdown numbered list syntax — never `1 ·` inline format.

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

### TABLE F — Common Activity Flags

*Note: TABLE F is defined here as a reference but not directly cited by role question sets (each role uses its own inline activity question list). Provided for documentation consistency only.*

1. I create customer presentations, decks, or visual materials
2. I run live demos connected to SAP systems
3. I write technical documents (RFPs, architecture specs, proposals)
4. I do value analysis, ROI calculations, or business cases
5. I research companies or build customer profiles before meetings
6. I develop or write code

---

## ROLE 1 — Solution Advisor / Solution Engineer

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

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces (Essential):** Ask ONE360, Product Knowledge (per AREAS), Melody Help, [Ask Services] Cloud ERP
**OneAI Spaces (Recommended):** ACTION — Meeting Minutes, [Ask Services] Value Acceleration
**GitHub Skills:** SAP Joule Module (Community) — github.com/vigneshbarani24/sap-superpowers (EXTERNAL only)
**AI-Generatable Templates:** T1 (SAP Product Expert, per AREAS), T2 (Presales Strategy Partner if PRESENTATIONS/DEMOS/RESEARCH), T4 (Demo Prep Assistant if DEMOS + DEMO_ENVS set)
**Personal Agent Template:** Role 1

---

## ROLE 2 — Enterprise Architect / Solution Architect

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

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces (Essential):** Product Knowledge BTM, Product Knowledge ERP, Product Knowledge Business Suite
**OneAI Spaces (Recommended):** ACTION — Integration Suite use case (if BAIP Tech Foundation)
**GitHub Skills:** SAP Joule Capability — github.com/SAP/custom-agentic-cookbook (EXTERNAL only)
**AI-Generatable Templates:** T3 (Application Design Coach if PRESENTATIONS flag)
**Personal Agent Template:** Role 1 base (substitute role name)

---

## ROLE 3 — Value Advisor / Value Engineer

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

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces (Essential):** Ask ONE360, [Ask Services] Cloud ERP, APM Pricing Agent
**OneAI Spaces (Recommended):** [Ask Services] Value Acceleration, Melody Help
**GitHub Skills:** None specific
**AI-Generatable Templates:** T2 (Presales Strategy Partner if PRESENTATIONS or RESEARCH) — *Role 3 is now included in T2 eligibility per SKILL.md update*
**Personal Agent Template:** Role 1 base (substitute role name)

---

## ROLE 4 — Delivery / Implementation Consultant

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

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces (Essential):** Product Knowledge ERP, BASE
**OneAI Spaces (Recommended):** Product Knowledge HCM (if SuccessFactors), Product Knowledge Spend (if Ariba)
**GitHub Skills:** None specific
**AI-Generatable Templates:** None (coding flag not typical for this role)
**Personal Agent Template:** Role 1 base (substitute role name)

---

## ROLE 5 — Data and AI Specialist / Analytics Architect

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

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces (Essential):** Golden Ticket to Data Products, Cloud Reporting
**OneAI Spaces (Recommended):** ACTION — Integration Suite use case, Product Knowledge ERP
**GitHub Skills:** Joule A2A Agent — github.com/The-Joule/sap-joule-ai-toolkit (if CODING); SAP Joule Capability — github.com/SAP/custom-agentic-cookbook (if CODING)
**AI-Generatable Templates:** T1 (SAP Product Expert, per PRODUCTS)
**Personal Agent Template:** Role 5

---

## ROLE 6 — Developer (BTP · CAP · ABAP · Integration)

### Questions

**Q1:** Which platform(s) do you primarily develop on? (TABLE E — multi-select)

**Q2:** Do you work with AI or LLM integration?

1. Yes (SAP AI Core, Generative AI Hub, vector search in SAP HANA Cloud)
2. No

*Type one number.*

**Q3:** Do you write technical documentation alongside your code?

1. Yes (API specs, READMEs, architecture docs)
2. No

*Type one number.*

Activity flags: PLATFORM (from Q1), AI_DEV(Q2=1), TECH_DOCS(Q3=1)
*Always assumed: coding=yes. Skipped: presentations, demos, value analysis, company research.*

### Skill Queries
Base: `ABAP`, `code review`, `CAP cloud`, `unit test`, `transport`, `API advisor`, `BTP developer`, `deploy BTP`
PLATFORM includes ABAP: + `ABAP clean core`, `ABAP unit test`, `S4HANA custom code`
PLATFORM includes BTP/CAP: + `CAP deployment`, `BTP cloud foundry`
PLATFORM includes Integration: + `CPI iFlow`, `integration suite`
PLATFORM includes SAP Build: + `SAP Build Code`, `process automation`
AI_DEV: + `AI Core`, `generative AI`, `vector HANA`
TECH_DOCS: + `technical writing`, `API documentation`, `README`

Note: developer skills have low community scores (0–3) but high role relevance. Do NOT apply a score > 10 filter for this role.

### MCP Queries
`jira`, `github`, `BTP`, `knowledge SAP`

### Personalization Defaults
- Always include working code examples; prefer complete snippets
- Concise technical answers; expand only when architecture context is needed
- Mention SAP-specific constraints (clean core, ABAP Cloud rules, BTP quotas)

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces (Essential):** ACTION — Integration Suite use case
**OneAI Spaces (Recommended):** Golden Ticket to Data Products (if AI_DEV)
**GitHub Skills:** Joule A2A Agent — github.com/The-Joule/sap-joule-ai-toolkit; SAP Joule Capability — github.com/SAP/custom-agentic-cookbook
**AI-Generatable Templates:** T3 (Application Design Coach — CODING flag is always set for Role 6, so T3 always applies)
**Personal Agent Template:** Role 1 base (substitute role name)

---

## ROLE 7 — Customer Success Manager / Adoption Specialist

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

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces (Essential):** Ask ONE360, CSM Process and Operations, AIgor — Adoption Programs
**OneAI Spaces (Recommended):** [Ask Services] Advanced Success Plan, [Ask Services] Max Success Plan, ASP Chatbot SharePoint, Global SPM/TQM Community
**GitHub Skills:** None specific
**AI-Generatable Templates:** None (CSM work is highly customer-contextual; no general template provides sufficient value)
**Personal Agent Template:** Role 7

---

## ROLE 8 — Product Manager / Industry Expert / SME

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

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces (Essential):** Product Knowledge BTM (if FOCUS=5)
**OneAI Spaces (Recommended):** Product Knowledge ERP, Product Knowledge Business Suite
**GitHub Skills:** Joule Studio 2.0 KB — github.tools.sap/SAP-Build-Enablement/Joule-Studio-and-SAP-Build-KB (INTERNAL, if FOCUS=4)
**AI-Generatable Templates:** None by default
**Personal Agent Template:** Role 1 base (substitute role name)

---

## ROLE 9 — Partner Consultant / Partner Sales Engineer

### Questions

**Q1:** Which SAP solutions do you specialize in? (TABLE A — multi-select)
- If AREAS includes 2 (BAIP) or 8 (All): ask **Q1-BAIP** (TABLE B — select one or more)
  - If TABLE B = 1 or 3: ask **Q1a** (TABLE C — multi-select)
  - If TABLE B = 2 or 3: ask **Q1b** (TABLE D — multi-select)

**Q2:** Which of these describe your work?

1. I run live customer demos or build proofs of concept
2. I develop custom solutions or integrations
3. Both
4. Neither

*Type one number.*

If Q2=2or3: **Q2a:** Which platform? (TABLE E — multi-select)

**Q3 — Demo Environments** (only ask if Q2 includes 1 or 3):
Build the numbered list using landscapes that intersect with SPECIALIZATION (AREAS from Q1). Apply the same LANDSCAPE FILTER as ROLE 1 (including the BTM-only skip rule). Re-number filtered options from 1. Always include "Multiple / I'll choose per customer" and "Not sure yet" as last options.

After selection, look up server endpoints in `references/demo-environments.md` filtered by SPECIALIZATION. Store as DEMO_ENVS.

Activity flags: SPECIALIZATION, BAIP_SUBAREAS, DEMOS(Q2=1or3), CODING(Q2=2or3), PLATFORM, DEMO_ENVS(from Q3)
*Always assumed: presentations=yes. Skipped: company research, internal SAP tools.*

### Skill Queries
Base: `demo guide`, `branded presentation`, `customer prep`, `selling coach`, `presales`, `customer engagement`
DEMOS: + `demo guide`, `WCI`, `demo`, `corporate pptx`
CODING ABAP: + `ABAP clean core`, `code review`
CODING BTP/CAP: + `CAP cloud`, `BTP deploy`
SPECIALIZATION includes CX: + `CRM`
SPECIALIZATION includes BAIP Data and AI: + `data visualization`, `generative AI`, `BDC advisor`
SPECIALIZATION includes BAIP Tech Foundation: + `BTP architecture`, `integration SAP`
SPECIALIZATION includes BTM: + `LeanIX`, `Signavio`, `WalkMe`

### MCP Queries
`experience`, `customer strategy`, `jira`, `github`

### Personalization Defaults
- Keep SAP product names precise — partners present to customers who notice naming errors
- Position SAP vs. competitors only on factual capability differences
- Clarify which licenses or entitlements are required for mentioned features

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces:** Not applicable (Role 9 is EXTERNAL/PARTNER mode — no OneAI access)
**GitHub Skills (PARTNER mode):** Joule A2A Agent — github.com/The-Joule/sap-joule-ai-toolkit (if CODING); SAP Joule Capability — github.com/SAP/custom-agentic-cookbook (if CODING); SAP Joule Module — github.com/vigneshbarani24/sap-superpowers
**AI-Generatable Templates:** T1 (SAP Product Expert, per SPECIALIZATION), T2 (Presales Strategy Partner if PRESENTATIONS/DEMOS), T4 (Demo Prep if DEMOS + DEMO_ENVS)
**Personal Agent Template:** Roles 9/12 template exists in agents.md — only usable if OneAI MCP is available (INTERNAL mode). PARTNER mode: not applicable at runtime.

---

## ROLE 10 — Business Development / Field Sales

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

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces (Essential):** Ask ONE360, Melody Help, [Ask Services] Cloud ERP
**OneAI Spaces (Recommended):** APM Pricing Agent, SAP HCM Commercial Desk
**GitHub Skills:** None specific
**AI-Generatable Templates:** T2 (Presales Strategy Partner if PRESENTATIONS or ACCOUNT_MGMT) — *Role 10 is now included in T2 eligibility per SKILL.md update*
**Personal Agent Template:** Role 1 base (substitute role name)

---

## ROLE 11 — Other / General

*For users whose role does not match any of the named options. Uses Role 1 (Solution Advisor) as a baseline and adapts based on the user's free-text description.*

### Questions

*(No structured questions — the user described their role in free text in Step A1 option 11.)*

Use the user's description to infer the closest matching role (1–10). If the description includes presales, advisory, or customer-facing work → use Role 1 base. If it includes technical or development work → use Role 6 base. If it includes delivery or implementation → use Role 4 base. Otherwise use Role 1 base.

Activity flags: infer from description. Default: PRESENTATIONS=false, DEMOS=false, TECH_DOCS=true.

### Skill Queries
Base: `selling coach`, `productivity`, `writing assistant`, `meeting notes`, `teams meeting`
If description includes demo/presentation: + `corporate pptx`, `demo guide`
If description includes technical/dev: + `BTP`, `ABAP`, `integration`

### MCP Queries
`knowledge SAP`, `experience`

### Personalization Defaults
- Use professional tone; adapt to the user's described context
- Do not assume SAP internal tools unless the user describes an SAP employee context
- Keep responses broad and adaptable until the user's domain becomes clearer

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces:** Inherit from closest matching role
**GitHub Skills:** None specific
**AI-Generatable Templates:** None by default
**Personal Agent Template:** Role 1 base (substitute role name from user description)

---

<!-- NOTE: Role 11 (Other) is defined above. Role 12 (Partner Delivery) follows below — it is a PARTNER-mode-only extension, not a standard numbered role in the internal list. -->

## SEARCH QUERY NOTES (from role testing)
- Use short queries (1-3 words). Multi-word SAP compounds return zero results.
- MCPs: single keywords only. `jira` works; `jira project` does not.
- Developer skills have scores 0-3 but are highly relevant — do not filter by score for Role 6.
- High-score generalist skills (score > 100) are suppressed from Essential for non-presentation roles.
- `customer engagement` and `knowledge management` return large pools (100+) — good catch-alls but require relevance filtering.
- Demo environment connectors are NOT searched via Hub — always resolve from demo-environments.md directly.
- BTM queries: `LeanIX`, `Signavio`, `WalkMe` work well; avoid compound terms like `enterprise architecture management`.
- When user selects option 8 (All) in TABLE A: treat as selecting all individual areas and trigger all applicable drill-downs, including BAIP (TABLE B).
- Customer track (Roles 13–17): use `web_search(query: "site:github.com/SAP/ai-skills-library <query>")`, fallback to `web_search(query: "SAP Joule skill <query>")` on 0 results. Always include direct link to skills.cloud.sap. Community scores are often 0–2 for business-user skills; do not apply score filter.
- Role 12 (PARTNER mode): also uses `web_search(query: "site:github.com/SAP/ai-skills-library <query>")` with skills.cloud.sap fallback (same approach as EXTERNAL mode).

---

## ROLE 12 — Partner Delivery / Implementation Consultant

*Available in PARTNER mode only (AUDIENCE = PARTNER). For partner consultants delivering SAP implementations or go-live projects at customer sites.*

### Questions

**Q1:** Which SAP solution(s) do you deliver for customers? (multi-select)

1. SAP Cloud ERP (SAP S/4HANA Cloud Public or Private Edition / RISE with SAP)
2. SAP SuccessFactors
3. SAP BTP and integrations (SAP Integration Suite, SAP Build, SAP CAP)
4. SAP Ariba / SAP Concur / SAP Fieldglass
5. SAP Business Data Cloud / SAP Datasphere / SAP Analytics Cloud
6. Multiple / All

*Type numbers separated by commas, or 'all'.*

**Q2:** Which of these describe your work?

1. Technical integration specs, architecture, or configuration documentation
2. Project delivery, go-live planning, and cutover activities
3. Building customer demos or proofs of concept
4. All of the above

*Type one number.*

If Q2=3 or 4: **Q3 — Demo Environments:** Apply Partner Mode Adjustment (check SIX access first). If SIX access confirmed, present landscape options filtered to SOLUTION from Q1 using the standard LANDSCAPE FILTER.

Activity flags: SOLUTION, TECH_DOCS (always assumed), PROJECT_MGMT(Q2=2or4), DEMOS(Q2=3or4)
*Skipped: value analysis. Always assumed: technical documentation=yes.*

### Skill Queries
Base: `project delivery`, `technical writing`, `go-live SAP`, `SAP documentation`, `implementation guide`, `meeting notes`
SOLUTION includes ERP: + `S4HANA`, `ABAP`
SOLUTION includes BTP: + `BTP deploy`, `integration suite`
SOLUTION includes SuccessFactors: + `SuccessFactors HR implementation`
SOLUTION includes BDC/Datasphere: + `data product`, `BDC advisor`
DEMOS: + `demo guide`, `customer prep`, `branded presentation`
PROJECT_MGMT: + `project checklist`, `cutover planning`

### MCP Queries
`jira`, `github`, `knowledge SAP`

### Personalization Defaults
- Be precise with SAP configuration paths, object names, and transport landscapes
- Use SAP Activate phase context where relevant (Discover, Prepare, Explore, Realize, Deploy, Run)
- Prefer numbered step-by-step formats for process guidance
- Clarify which licenses and entitlements are required for customer-facing features

### Agent & GitHub Catalog (v1.8.0)
**OneAI Spaces:** Not applicable (PARTNER mode — no OneAI access)
**ADAPT Connector:** Always included per ADAPT Connector Rule (Roles 1, 9, 12 in PARTNER mode).
**GitHub Skills (PARTNER mode):** Joule A2A Agent — github.com/The-Joule/sap-joule-ai-toolkit (if SOLUTION includes BTP); SAP Joule Capability — github.com/SAP/custom-agentic-cookbook (if SOLUTION includes BTP)
**AI-Generatable Templates:** T4 (Demo Prep if DEMOS + DEMO_ENVS)
**Personal Agent Template:** Roles 9/12 template exists in agents.md — only usable if OneAI MCP is available. PARTNER mode: not applicable at runtime (same as Role 9).

---

## ROLE 13 — Customer IT Professional / Joule Work Administrator

*For IT professionals at SAP customer companies responsible for Joule Work setup and SAP system connectivity.*

### Questions

**Q1:** What is your primary responsibility?

1. Configure and maintain Joule Work for my team or organisation
2. Connect Joule Work to our SAP systems
3. General IT support, productivity tools, and internal automation
4. All of the above

*Type one number.*

**Q2:** Which SAP systems does your organisation use? (multi-select)

1. SAP Cloud ERP (SAP S/4HANA Cloud / RISE with SAP)
2. SAP SuccessFactors
3. SAP Ariba / SAP Concur
4. SAP Business Data Cloud / SAP Datasphere / SAP Analytics Cloud
5. SAP BTP services
6. None of the above / I am not sure

*Type numbers separated by commas, or 'all'.*

Activity flags: CONFIGURE(Q1=1or4), CONNECT(Q1=2or4), IT_SUPPORT(Q1=3or4), SYSTEMS (from Q2)

### Skill Queries
Base: `productivity tools`, `IT assistant`, `governance`, `joule setup`
CONNECT: + `SAP integration`, `API connectivity`, `connector`
SYSTEMS includes ERP: + `S4HANA`, `RISE with SAP`
SYSTEMS includes SuccessFactors: + `SuccessFactors HR`
SYSTEMS includes BDC: + `data product`, `analytics`
CONFIGURE: + `joule configuration`, `admin setup`
IT_SUPPORT: + `workflow automation`, `process automation`

### MCP Queries
*(Customer Track: CT2 does not invoke `search_mcp_servers`. These queries are for reference only and are not executed at runtime.)*
`knowledge SAP`, `governance`

### Personalization Defaults
- Keep answers practical and implementation-focused
- Include exact menu paths and setting names for Joule Work configuration
- Use plain language; avoid SAP presales or sales framing

---

## ROLE 14 — Customer Business User — Finance / Accounting

*For finance and accounting professionals at SAP customer companies.*

### Questions

**Q1:** What is your finance role focus? (multi-select)

1. Financial planning and analysis (FP&A, budgeting, forecasting)
2. Accounting and period-close activities
3. Treasury and cash management
4. Expense and spend management
5. Financial reporting and analytics
6. All of the above

*Type numbers separated by commas, or 'all'.*

**Q2:** Do you work with any of these SAP tools?

1. SAP Analytics Cloud (dashboards, reports, planning)
2. SAP S/4HANA Finance (journal entries, cost centres)
3. SAP Concur (expense reports, travel)
4. None of the above

*Type one number.*

Activity flags: FOCUS (from Q1), SAP_TOOLS (from Q2)

### Skill Queries
Base: `finance assistant`, `financial analysis`, `reporting`, `budgeting`, `meeting notes`
FOCUS includes FP&A: + `financial planning`, `forecast`
FOCUS includes expense: + `expense management`, `travel`
FOCUS includes analytics: + `data visualization`, `analytics`
SAP_TOOLS includes SAC: + `SAP Analytics Cloud`, `SAC report`
SAP_TOOLS includes Concur: + `expense`, `SAP Concur`

### MCP Queries
*(Customer Track: CT2 does not invoke `search_mcp_servers`. These queries are for reference only and are not executed at runtime.)*
`analytics`, `knowledge SAP`

### Personalization Defaults
- Lead with clear numbers and outcomes; avoid unnecessary technical detail
- Flag when a number is an estimate vs. confirmed data
- Use professional finance terminology; match the user's level of detail

---

## ROLE 15 — Customer Business User — HR / People Management

*For HR and people management professionals at SAP customer companies.*

### Questions

**Q1:** What is your HR focus? (multi-select)

1. Talent acquisition and recruitment
2. Employee experience and engagement
3. Learning and development
4. HR operations and administration
5. Workforce analytics and planning
6. All of the above

*Type numbers separated by commas, or 'all'.*

**Q2:** Do you use SAP SuccessFactors?

1. Yes — it is our core HR system
2. Partially — some modules
3. No

*Type one number.*

Activity flags: FOCUS (from Q1), SF(Q2=1or2)

### Skill Queries
Base: `HR assistant`, `people management`, `recruitment`, `employee experience`, `meeting notes`
FOCUS includes recruitment: + `talent acquisition`, `candidate screening`
FOCUS includes analytics: + `workforce analytics`, `HR reporting`
FOCUS includes learning: + `learning development`
SF: + `SuccessFactors`, `HCM assistant`

### MCP Queries
*(Customer Track: CT2 does not invoke `search_mcp_servers`. These queries are for reference only and are not executed at runtime.)*
`knowledge SAP`

### Personalization Defaults
- Use plain language accessible to HR generalists and specialists alike
- Balance empathy with operational precision in communications
- Avoid SAP internal jargon; use standard HR industry terms

---

## ROLE 16 — Customer Business User — Operations / Supply Chain

*For operations, procurement, logistics, and supply chain professionals at SAP customer companies.*

### Questions

**Q1:** What is your operations focus? (multi-select)

1. Procurement and sourcing
2. Logistics and warehouse management
3. Manufacturing and production
4. Supply chain planning and forecasting
5. Sustainability and ESG tracking
6. All of the above

*Type numbers separated by commas, or 'all'.*

**Q2:** Do you use any of these SAP tools?

1. SAP Ariba (procurement and sourcing)
2. SAP Integrated Business Planning (supply chain planning)
3. SAP S/4HANA (operations modules)
4. SAP Analytics Cloud (reporting and dashboards)
5. None / Other tools

*Type one number.*

Activity flags: FOCUS (from Q1), SAP_TOOLS (from Q2)

### Skill Queries
Base: `supply chain`, `procurement assistant`, `operations`, `logistics`, `meeting notes`
FOCUS includes procurement: + `procurement sourcing`, `SAP Ariba`
FOCUS includes planning: + `supply chain planning`, `IBP`
FOCUS includes sustainability: + `sustainability ESG`, `green ledger`
SAP_TOOLS includes Ariba: + `Ariba`, `procurement`
SAP_TOOLS includes IBP: + `IBP planning`

### MCP Queries
*(Customer Track: CT2 does not invoke `search_mcp_servers`. These queries are for reference only and are not executed at runtime.)*
`knowledge SAP`

### Personalization Defaults
- Be precise with operational quantities, dates, and process steps
- Use supply chain industry terminology
- Flag dependencies between steps (e.g. procurement blocked until approval)

---

## ROLE 17 — Customer Business User — Data Analyst / General Knowledge Worker

*For data analysts, BI users, and general knowledge workers at SAP customer companies.*

### Questions

**Q1:** Which of these best describe your daily work? (multi-select)

1. Meetings, communications, and team coordination
2. Data analysis, dashboards, and reporting
3. Document creation, research, and writing
4. Customer or stakeholder engagement
5. Process automation or workflow management

*Type numbers separated by commas, or 'all'.*

Activity flags: FOCUS (from Q1)

### Skill Queries
Base: `productivity`, `writing assistant`, `meeting notes`, `research`
FOCUS includes data (Q1=2): + `data visualization`, `analytics`, `BI report`
FOCUS includes communications (Q1=1): + `email management`, `meeting intelligence`
FOCUS includes documents (Q1=3): + `document creation`, `presentation`
FOCUS includes customer engagement (Q1=4): + `customer engagement`, `stakeholder communication`
FOCUS includes automation (Q1=5): + `process automation`, `workflow`

### MCP Queries
*(Customer Track: CT2 does not invoke `search_mcp_servers`. These queries are for reference only and are not executed at runtime.)*
`knowledge SAP`

### Personalization Defaults
- Adapt to the user's domain; do no