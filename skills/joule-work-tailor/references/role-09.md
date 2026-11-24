# Role 9 — Partner Consultant / Partner Sales Engineer

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

### Agent & GitHub Catalog
**OneAI Spaces:** Not applicable (Role 9 is EXTERNAL/PARTNER mode — no OneAI access)
**GitHub Skills (PARTNER mode):** Joule A2A Agent — github.com/The-Joule/sap-joule-ai-toolkit (if CODING); SAP Joule Capability — github.com/SAP/custom-agentic-cookbook (if CODING); SAP Joule Module — github.com/vigneshbarani24/sap-superpowers
**AI-Generatable Templates:** T1 (SAP Product Expert, per SPECIALIZATION), T2 (Presales Strategy Partner if PRESENTATIONS/DEMOS) — see agents.md Section 3 for full T2 role eligibility; T4 (Demo Prep if DEMOS + DEMO_ENVS)
**Personal Agent Template:** Roles 9/12 template exists in agents.md — only usable if OneAI MCP is available (INTERNAL mode). PARTNER mode: not applicable at runtime.

---


---

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