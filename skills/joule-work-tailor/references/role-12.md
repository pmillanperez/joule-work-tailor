# Role 12 — Partner Delivery / Implementation Consultant

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

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

### Agent & GitHub Catalog
**OneAI Spaces:** Not applicable (PARTNER mode — no OneAI access)
**ADAPT Connector:** Always included per ADAPT Connector Rule (Roles 1, 9, 12 in PARTNER mode).
**GitHub Skills (PARTNER mode):** Joule A2A Agent — github.com/The-Joule/sap-joule-ai-toolkit (if SOLUTION includes BTP); SAP Joule Capability — github.com/SAP/custom-agentic-cookbook (if SOLUTION includes BTP)
**AI-Generatable Templates:** T4 (Demo Prep if DEMOS + DEMO_ENVS)
**Personal Agent Template:** Roles 9/12 template exists in agents.md — only usable if OneAI MCP is available. PARTNER mode: not applicable at runtime (same as Role 9).

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