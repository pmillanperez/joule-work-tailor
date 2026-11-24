# Role 6 — Developer (BTP · CAP · ABAP · Integration)

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

## Shared Tables Referenced

### TABLE E — Developer Platforms

1. ABAP (on-premise or ABAP Cloud)
2. SAP BTP & CAP (Cloud Application Programming Model)
3. SAP Integration Suite (iFlows, adapters, API proxies)
4. SAP Build (Apps, Process Automation, Code)
5. Multiple / All

---

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

### Agent & GitHub Catalog
**OneAI Spaces (Essential):** ACTION — Integration Suite use case
**OneAI Spaces (Recommended):** Golden Ticket to Data Products (if AI_DEV)
**GitHub Skills:** Joule A2A Agent — github.com/The-Joule/sap-joule-ai-toolkit; SAP Joule Capability — github.com/SAP/custom-agentic-cookbook
**AI-Generatable Templates:** T3 (Application Design Coach — CODING flag is always set for Role 6, so T3 always applies)
**Personal Agent Template:** Role 1 base (substitute role name)

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