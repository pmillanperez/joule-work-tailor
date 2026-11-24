# Role 11 — Other / General

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

---

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

### Agent & GitHub Catalog
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
