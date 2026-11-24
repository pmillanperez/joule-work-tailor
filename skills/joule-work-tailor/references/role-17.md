# Role 17 — Customer Business User — Data Analyst / General Knowledge Worker

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

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