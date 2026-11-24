# Role 15 — Customer Business User — HR / People Management

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

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
