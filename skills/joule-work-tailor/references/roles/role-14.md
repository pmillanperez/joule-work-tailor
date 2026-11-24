# Role 14 — Customer Business User — Finance / Accounting

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

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
