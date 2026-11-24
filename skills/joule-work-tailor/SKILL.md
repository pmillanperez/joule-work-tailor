---
name: joule-work-tailor
description: >-
  Role-based Joule Work Tailor skill for SAP employees, partner consultants, and SAP customer business users. Covers 17 roles across Cloud ERP, BAIP, CX, Supply Chain, Finance, HCM, BTM, and customer business functions. Two modes: first-time setup and stay current. Three audiences: SAP internal (hub.joule.only.sap), partner (public catalog + partner track), customer (simplified productivity track). ADAPT connector auto-surfaced for advisory roles. Activate: Joule Work Tailor, joule-work-tailor, equip my JWD, tailor JWD to my role, tailor my Joule Work, set up Joule Work for my role, personalize joule work, what skills should I install, set up my demo environments, which demo connectors should I install, set up Joule as a partner, I work at an SAP customer, personaliza mi joule work, qué necesito para mi rol, JWD einrichten, Joule für meine Rolle anpassen, Joule Work personalisieren, welche Skills brauche ich.
allowed-tools: create_skill install_asset write_file web_search create_space add_card_to_space list_emails list_calendar_events read_file
metadata:
  author: Pedro Millan
  version: 3.0.1
  tags: setup onboarding presales role-based demo-environments toolkit partner customer personalization skills-management state-management scoring outlook-enrichment phase-split
  required-mcp-servers: "*hub.joule.only.sap*"
---

# Joule Work Tailor

Personalise Joule Work Desktop with the right skills, connectors, demo environments, and profile settings for your role. The user never has to type product names or free-text descriptions — every question is a numbered option list answered with a single number or a comma-separated list.

**Architecture:** This skill uses phase-based loading to minimize response time. This file contains routing logic and global rules only. Detailed instructions for each phase are in separate files under `references/` — loaded on demand with `read_file`. Do NOT read all phase files at once; load only the one needed for the current step.


Three audiences are supported: **SAP employees** (internal hub), **SAP partner consultants** (public catalog + partner-specific guidance), and **SAP customer business users** (public catalog + customer role set).

Two modes: **first-time setup** or **stay current** with updates and new content.

---

## CRITICAL OUTPUT FORMATTING — APPLIES TO EVERY QUESTION IN THIS SKILL

All option lists MUST use **standard markdown numbered list syntax**. This is a hard rule — it guarantees each option renders on its own line.

Required format:

**Question text?**

1. Option one
2. Option two
3. Option three

*Type one number.*

Do NOT use inline format (`1 · Option one 2 · Option two`). Do NOT use bold-number format (`**1** · Option`). ALWAYS use `1.` `2.` `3.` markdown list syntax so each option is on its own line. This applies to every question throughout the entire session.

---

## State Management — Persistent Memory

This skill maintains a state file across conversations to remember the user's role, preferences, and past decisions. Returning users get instant startup — no repeated questions — and smarter recommendations that improve over time.

**State file:** `joule-work-tailor/joule-work-tailor-state.json`

**Schema:**

```json
{
  "schema_version": 1,
  "last_run": "ISO timestamp",
  "last_run_mode": "setup | update",
  "audience": "INTERNAL | PARTNER | CUSTOMER",
  "role": "Solution Advisor",
  "role_number": 1,
  "secondary_role": "Developer",
  "secondary_role_number": 6,
  "areas": ["Cloud ERP", "Data and AI"],
  "flags": { "DEMOS": true, "PRESENTATIONS": true, "CODING": false, "RESEARCH": true },
  "demo_envs": ["C39", "GLA-26Q2"],
  "language": "en",
  "installed_uuids": ["uuid1", "uuid2"],
  "installed_connectors": ["ADAPT Connector"],
  "last_seen_catalog": ["uuid-all-seen-on-last-run"],
  "skipped_skills": [{ "uuid": "x", "version": "1.2", "name": "skill-name", "skipped_at": "ISO" }],
  "dismissed_skills": ["uuid-permanently-hidden"],
  "profile_enriched": true,
  "last_enriched": "ISO timestamp or null"
}
```

`secondary_role` and `secondary_role_number` are optional — omit (or set to `null`) when no secondary role was selected.

**Read points:** Step 0 (init) · Step A3 (NEW detection) · Step A5 (filter dismissed/skipped) · Flow B (reuse profile).
**Write points:** Step A5 (last_seen_catalog) · Step A6 end (full state) · Flow B end (full state) · Dismiss command (dismissed_skills).

**Retrocompatibility:** If `joule-work-tailor/joule-work-tailor-state.json` does not exist but `my-joule-work-tailor.md` or `joule-work-tailor/my-joule-work-tailor.md` does, parse the profile file to infer role and areas from the ROLE DESCRIPTION block. Pre-populate a new state with whatever can be extracted. All list fields (installed_uuids, last_seen_catalog, skipped_skills, dismissed_skills) default to empty arrays. `secondary_role` and `secondary_role_number` default to `null` when absent from an older state file.

**Role change escape hatch:** At any point during the session, if the user says "change my role", "update my role", "cambiar mi rol", "Rolle ändern", or similar → re-run Step A1 → A1b → A2 → A2b with the full question flow, update the state file, and continue from where they left off. If the user says "change my secondary role", "remove secondary role", or similar → re-run Step A1b only (skip A1 and A2).

---

## Step 0 — Initialize Session

**Step 0-pre — Silent state check (no output to user):**

Fire in a single parallel message:
1. `read_file` → `joule-work-tailor/joule-work-tailor-state.json`
2. `ls` → check `joule-work-tailor/` directory for `my-joule-work-tailor.md`

Determine session context:
- **STATE = loaded** — state file found and contains a `role` field. Extract all fields into session variables.
- **STATE = migrated** — state file not found, but `joule-work-tailor/my-joule-work-tailor.md` found (or legacy `my-joule-work-tailor.md` in root). Read it with `read_file`. Parse the ROLE DESCRIPTION block to infer role name and areas. Map to the closest role number from the role list. Set all list fields (installed_uuids, last_seen_catalog, skipped_skills, dismissed_skills) to empty arrays. Create a new in-memory state object.
- **STATE = fresh** — neither file found. First-time user.

Do NOT output anything to the user from this step. Proceed silently to Step 0a.

> ⚠️ **Context note:** If you are currently chatting from inside another Joule Space (e.g. a customer project space), Joule Work Tailor may add cards to that space unintentionally. To keep your toolkit separate, run this skill from the **main Joule chat** — not from inside a project Space. At the end of setup, you can create a dedicated **'My JWD Toolkit'** space.

**Step 0a — Catalog detection:**
- `mcp_<hash>_search_skills` tool available → **INTERNAL mode** (SAP employees on hub.joule.only.sap)
- Not available → **EXTERNAL mode**

**Step 0b — Audience detection (EXTERNAL mode only):**

Ask:

**One question to personalise your experience:**

1. I work at an SAP partner firm (consultant, sales engineer, architect, or similar)
2. I work at an SAP customer company (Finance, HR, IT, Operations, or other business role)
3. Other / prefer not to say

*Type one number.*

Set AUDIENCE = PARTNER (1), CUSTOMER (2), or GENERAL (3).

**If AUDIENCE = CUSTOMER:** skip Flow A and Flow B entirely — jump to the **CUSTOMER TRACK** section below.

**Announce mode:**
- INTERNAL: *"I'll search the SAP internal skills hub."*
- PARTNER: *"I'll search the public SAP AI Skills Library (skills.cloud.sap) and tailor recommendations for partner consultants."*
- GENERAL: *"I'll search the public SAP AI Skills Library (skills.cloud.sap). Some SAP-internal skills may not be available."*

If INTERNAL, offer:

*Should I also check skills.cloud.sap for additional public skills?*

1. Yes
2. No

**Step 0c — Returning user fast path:**

If STATE = loaded or STATE = migrated:
- If audience is stored in state, reuse it silently (skip Step 0b entirely for EXTERNAL users).
- Do NOT ask role questions — proceed directly to Step 1 with stored context.
- The role/areas/flags/demo_envs from state will be used in all subsequent steps.

If STATE = fresh:
- Continue normally through Step 0b (if EXTERNAL) and Step 1.

---

## Step 1 — Choose Mode

**If STATE = loaded or STATE = migrated (returning user):**

Announce briefly — one line, no questions:

*"Running as **[ROLE]**[+ **[SECONDARY_ROLE]**] — [AREAS as comma-separated list]. Say 'change my role' anytime."*

(Include the secondary role in brackets only if `HAS_SECONDARY = true`)

Then ask:

**What would you like to do?**

1. Refresh my toolkit — discover new skills and check for updates
2. Full setup — reconfigure everything from scratch

*Type one number.*

Route: 1 → Flow B (Stay Current, using stored context). 2 → Flow A (full setup — re-run A1 → A2 → A2b with questions, then continue to A3).

**If STATE = fresh (first-time user):**

**Which situation are you in?**

1. Set up my toolkit — find and install the right skills and connectors for my role
2. Stay current — check for updates and discover new content I'm missing

*Type one number.*

Route to Flow A (answer 1) or Flow B (answer 2).

---


---

## Phase Routing

After determining the flow (A, B, or Customer Track), load ONLY the relevant phase file with `read_file`. Load phase files one at a time as the flow progresses — never all at once.

### Flow A — First-Time Setup

| Phase | When to load | File |
|-------|-------------|------|
| **Intake** | After Step 1 routes to Flow A | `read_file("references/phase-intake.md")` then `read_file("references/role-{NN}.md")` where NN = zero-padded role number from A1 |
| **Search** | After A2b confirmation completes | `read_file("references/phase-search.md")` — the role file is already in context from Intake |
| **Output** | After A3 search results are ready | `read_file("references/phase-output.md")` |

### Flow B — Stay Current

After Step 1 routes to Flow B:
1. `read_file("references/phase-flow-b.md")`
2. `read_file("references/role-{NN}.md")` using stored role number from state

### Customer Track

After Step 0b sets AUDIENCE = CUSTOMER:
1. `read_file("references/phase-customer.md")`
2. After CT1 role selection: `read_file("references/role-{NN}.md")` using customer role mapping (CT1 option 1 = role-14, 2 = role-15, 3 = role-16, 4 = role-13, 5 = role-17, 6 = role-17)

### Role File Naming

Files: `role-01.md` through `role-17.md` in `references/`. See `references/roles-index.md` for the full mapping.

**IMPORTANT:** Do NOT read the monolithic `roles-archive.md` in the skill root — it is 40KB and contains all 17 roles. Always read the individual `role-{NN}.md` file for the user's selected role.

### Other Reference Files (read on demand, not upfront)

- `references/agents.md` — OneAI spaces, GitHub skills, AI templates, personal agent templates. Read during phase-search and phase-output.
- `references/demo-environments.md` — SIX landscape tables. Read during phase-intake (Q3) and phase-search (connector lookup).
- `references/sap-portfolio.md` — Official SAP product names. Read during phase-output (profile generation).

## Dismiss Command

When the user says "dismiss N", "dismiss 3, 7", "hide N", "ocultar N", "ausblenden N", or similar at any point after recommendations are shown:

1. Parse the number(s). Look up the corresponding UUID(s) from the most recent recommendations (the numbered rows from Step A5 or B4).
2. For each UUID: add to `dismissed_skills` in the state object.
3. Save updated state to `joule-work-tailor/joule-work-tailor-state.json` immediately (read current file → merge dismissed list → write).
4. Confirm: *"Hidden — [skill name] won't appear again."*

**Undo dismiss:** If the user says "undo dismiss", "undo dismiss N", or "show dismissed":
- Specific number/name: remove that UUID from `dismissed_skills`, save state, confirm.
- General ("show dismissed"): list all dismissed skill names. The user can say "restore N" to un-dismiss.

**Important:** Dismiss does NOT uninstall — it only hides from future recommendations.

---

## Manual Install Reference
- **Skills:** Extensions → Skills → New → Upload → select `.zip`
- **Connectors:** Extensions → Connectors → New → Add Connector → paste URL and name
- **Updates:** Extensions → Skills → select skill → More → Check for Updates → Update
- **Public catalog (all users):** skills.cloud.sap
- **Internal hub (SAP employees):** hub.joule.only.sap
- **SIX demo hub:** https://six-mcp-hub.cfapps.eu10-004.hana.ondemand.com/#landscapes
- **OneAI (SAP employees):** https://oneai.cfapps.eu10-004.hana.ondemand.com
- **OAuth error fix:** close ALL browser tabs → log in fresh at experience-agent.cloud.sap → retry connector install
