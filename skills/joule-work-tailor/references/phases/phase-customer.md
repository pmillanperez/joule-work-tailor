# Phase: Customer Track (CT0–CT5)

This file is loaded when AUDIENCE = CUSTOMER. Read it with `read_file` at the start of the Customer Track.

---

## CUSTOMER TRACK

*For users at SAP customer companies using Joule Work as a daily productivity and AI tool.*

### CT0 — Returning Customer Fast Path

**If STATE = loaded and `audience` = "CUSTOMER":**

Announce briefly — one line:

*"Running as **[ROLE name]**. Say 'change my role' anytime."*

Then ask:

**What would you like to do?**

1. Refresh my toolkit — discover new skills since last time
2. Full setup — reconfigure everything from scratch

*Type one number.*

**If 1 → Customer Refresh (CT-B):**
1. Run CT2 catalog search using stored CUSTOMER_ROLE and areas.
2. Apply state-aware filters (same as A3 steps 9–12, including the fallback chain):
   - **NEW classification:** compare result UUIDs against `last_seen_catalog` from state. Mark new with 🆕.
   - **Filter dismissed:** remove any UUID in `dismissed_skills`.
   - **Suppress skipped at same version:** suppress skills in `skipped_skills` at the same version. Re-offer (🔄) if version changed.
   - **Fallback chain:** if < 3 results after filtering, run Fallback A → B → C.
3. Present update report (same format as B4 — UPDATES AVAILABLE + NEW SKILLS sections, source legend, dismiss hint).
4. Install CTA: Update everything / Choose / Done.
5. Save state (same logic as CT4 state save — update `last_seen_catalog`, merge `installed_uuids`, update `skipped_skills`). Set `last_run_mode` to `"update"`.

**If 2 → Full setup:** re-run CT1 → CT2 → CT3 → CT4 → CT5 with full question flow, overwriting stored state.

**If STATE = fresh or STATE = migrated:** continue to CT1 normally.

### CT1 — Role

**What best describes your role?**

1. Finance / Accounting (budgeting, reporting, expense, treasury)
2. HR / People Management (recruitment, workforce, employee experience)
3. Operations / Supply Chain (procurement, logistics, planning, manufacturing)
4. IT / System Administrator (Joule Work setup and SAP system connectivity)
5. Data Analyst / BI User (dashboards, reports, data-driven decisions)
6. General Knowledge Worker / Other

*Type one number.*

Map to role file: 1→role-14.md, 2→role-15.md, 3→role-16.md, 4→role-13.md, 5→role-17.md, 6→role-17.md. Store as CUSTOMER_ROLE. Load the corresponding role file with `read_file("references/roles/role-{NN}.md")`.

### CT2 — Catalog Search

Search the public catalog: `web_search(query: "site:github.com/SAP/ai-skills-library <query>")` using base queries for CUSTOMER_ROLE from the role file already in context.

**Retry logic:** If a query returns 0 results, retry with the last word removed. If still 0, fallback to `web_search(query: "SAP Joule skill <query>")`. If all fallbacks return 0, skip that query silently. After all searches, always append: *"Browse the full public catalog at [skills.cloud.sap](https://skills.cloud.sap)."*

**Do NOT include:**
- SIX demo environment connectors
- SAP-internal MCP endpoints
- Skills requiring SAP employee hub access
- Presales or deal-centric skills (selling coach, account intelligence, ADAPT, etc.)
- GitHub skills or AI-generatable skills (too technical for customer business user track)

Cap: 6 Essential + 8 Recommended + 4 Optional skills.

### CT3 — Show Recommendations and Profile

Generate profile for CUSTOMER_ROLE (same logic as A5, using role defaults from the role file already in context). In Role Description: identify role and company type (e.g. "Finance professional at an SAP customer company"). Do not use SAP internal role taxonomy or presales jargon.

Show profile blocks at the top, using the same format and invite sentence as Step A5 (profile first, recommendations below).

Save as `joule-work-tailor/my-joule-work-tailor.md` using `write_file`. Omit demo connectors, GitHub, AI-generatable, and OneAI sections.

### CT4 — Install

Use EXTERNAL install path: provide numbered download links and manual steps (Extensions → Skills → New → Upload → select `.zip`).

**How would you like to proceed?**

1. Install Essential Bundle (manual steps)
2. Choose items manually
3. I’m done — I’ll install later

End: *"Your Joule Work is now set up for your role. Open a new conversation to start using your new skills."*


**Save state (silent — no output to user):**

After the install summary, save the full state to `joule-work-tailor/joule-work-tailor-state.json` using `write_file`. Build the JSON object with:
- `schema_version`: 1
- `last_run`: current ISO timestamp
- `last_run_mode`: "setup"
- `audience`: "CUSTOMER"
- `role`: CUSTOMER_ROLE name from CT1
- `role_number`: CUSTOMER_ROLE number (13–17)
- `areas`: inferred from role (e.g. Role14 = ["Finance"])
- `flags`: empty object (Customer Track does not collect activity flags)
- `demo_envs`: empty array
- `language`: LANGUAGE detected from conversation
- `installed_uuids`: UUIDs of skills successfully installed in this session
- `installed_connectors`: empty array
- `last_seen_catalog`: all skill UUIDs found in CT2 search results
- `skipped_skills`: skills shown in CT3 but NOT installed
- `dismissed_skills`: carry forward from existing state (only modified by the dismiss command)
- `profile_enriched`: false
- `last_enriched`: null

Write as formatted JSON (indent 2). Do not mention the state save to the user.

### CT5 — Optional: Save to Joule Space

After install, offer once:

**Before calling `create_space`:** Check if the current conversation is running inside an existing Space. If yes, warn the user: *"You are currently in a Joule Space. Creating a new 'My JWD Toolkit' space will add it to your Spaces panel — it will NOT affect the current space. Continue?"* Present as: `1. Yes — create the new space` / `2. No — skip`. Only proceed if confirmed.

---

**Would you like me to create a 'My JWD Toolkit' space to save your recommendations?**

1. Yes — create a space for my toolkit
2. No — the results are saved in `joule-work-tailor/my-joule-work-tailor.md`

*Type one number.*

---

If 1: follow the same flow as Step A8 above, adapted for the customer role name.

---

