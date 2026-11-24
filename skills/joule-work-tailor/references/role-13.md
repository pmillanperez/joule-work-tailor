# Role 13 — Customer IT Professional / Joule Work Administrator

## HOW QUESTIONS WORK

Every question uses **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types only numbers.
- Multi-select: end with "Type numbers separated by commas, or 'all'."
- Single-select: end with "Type one number."
- Yes/No: present as a two-item list: `1. Yes` / `2. No`.
- Drill-down: only ask the sub-question if the parent answer triggers it.
- Never ask the user to type a product name.

---

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
