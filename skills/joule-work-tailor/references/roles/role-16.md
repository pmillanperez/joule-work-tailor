# Role 16 — Customer Business User — Operations / Supply Chain

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

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
