---
name: joule-work-tailor
description: >-
  Role-based Joule Work Tailor skill for SAP employees, partner consultants, and SAP customer business users. Covers 17 roles across Cloud ERP, BAIP, CX, Supply Chain, Finance, HCM, BTM, and customer business functions. Two modes: first-time setup and stay current. Three audiences: SAP internal (hub.joule.only.sap), partner (public catalog + partner track), customer (simplified productivity track). ADAPT connector auto-surfaced for advisory roles. Activate: Joule Work Tailor, joule-work-tailor, equip my JWD, tailor JWD to my role, tailor my Joule Work, set up Joule Work for my role, personalize joule work, what skills should I install, set up my demo environments, which demo connectors should I install, set up Joule as a partner, I work at an SAP customer, personaliza mi joule work, qué necesito para mi rol, JWD einrichten, Joule für meine Rolle anpassen, Joule Work personalisieren, welche Skills brauche ich.
allowed-tools: create_skill install_asset write_file web_search create_space add_card_to_space list_emails list_calendar_events read_file
metadata:
  author: Pedro Millan
  version: 2.0.3
  tags: setup onboarding presales role-based demo-environments toolkit partner customer personalization skills-management state-management scoring outlook-enrichment
  required-mcp-servers: "*hub.joule.only.sap*"
---

# Joule Work Tailor

Personalise Joule Work Desktop with the right skills, connectors, demo environments, and profile settings for your role. The user never has to type product names or free-text descriptions — every question is a numbered option list answered with a single number or a comma-separated list.

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

**State file:** `joule-work-tailor-state.json` (working directory)

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

**Retrocompatibility:** If `joule-work-tailor-state.json` does not exist but `my-joule-work-tailor.md` does, parse the profile file to infer role and areas from the ROLE DESCRIPTION block. Pre-populate a new state with whatever can be extracted. All list fields (installed_uuids, last_seen_catalog, skipped_skills, dismissed_skills) default to empty arrays. `secondary_role` and `secondary_role_number` default to `null` when absent from an older state file.

**Role change escape hatch:** At any point during the session, if the user says "change my role", "update my role", "cambiar mi rol", "Rolle ändern", or similar → re-run Step A1 → A1b → A2 → A2b with the full question flow, update the state file, and continue from where they left off. If the user says "change my secondary role", "remove secondary role", or similar → re-run Step A1b only (skip A1 and A2).

---

## Step 0 — Initialize Session

**Step 0-pre — Silent state check (no output to user):**

Fire in a single parallel message:
1. `read_file` → `joule-work-tailor-state.json`
2. `ls` → check working directory for `my-joule-work-tailor.md`

Determine session context:
- **STATE = loaded** — state file found and contains a `role` field. Extract all fields into session variables.
- **STATE = migrated** — state file not found, but `my-joule-work-tailor.md` found. Read it with `read_file`. Parse the ROLE DESCRIPTION block to infer role name and areas. Map to the closest role number from the role list. Set all list fields (installed_uuids, last_seen_catalog, skipped_skills, dismissed_skills) to empty arrays. Create a new in-memory state object.
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

## Partner Mode Adjustments

When AUDIENCE = PARTNER (EXTERNAL), apply these overrides throughout Flow A:

**Role list (Step A1):** Show all 11 existing roles. Add one more:
- **12. Partner Delivery / Implementation Consultant** — delivery, go-live, and integration work at customer sites

**Secondary role (Step A1b):** Also available in PARTNER mode. Show the same extended 12-role list (minus the already-chosen primary). The "No secondary role" option is always last.

**Demo environments (Step A3 Q3 — Roles 1, 5, 9, 12 with DEMOS flag):**
Before showing landscape options, ask:

*"Do you have access to SAP SIX partner demo environments?"*
1. Yes — I have SIX partner access
2. No — I use the customer's SAP tenant or will request partner demo access

If 2: skip the landscape picker. Inform: *"For demos without SIX access, use your customer's SAP system or contact your SAP partner contact for a demo tenant. You can still install the Experience Agent connector."* Set DEMO_ENVS = none.
If 1: continue with the standard landscape selection from `references/demo-environments.md`.

**ADAPT connector (Step A5, Roles 1, 9, 12):** Apply the ADAPT Connector Rule below.

**Install flow (Step A6):** Always use EXTERNAL path. First attempt GitHub auto-install via `create_skill` (Step 1 of the EXTERNAL flow in Step A6). Fall back to manual steps only if auto-install fails.

---

## ADAPT Connector Rule

For the following role + mode combinations, **always** include the ADAPT connector in ESSENTIAL CONNECTORS — prepend it to the connector list even if Hub search did not return it:
- Roles 1, 7, 9, 10 in **INTERNAL** mode
- Roles 1, 9, 12 in **PARTNER** mode

```
| Name | Source | What it does | URL |
|------|--------|-------------|-----|
| ADAPT Connector | SAP Official | Repository of Extended AI use cases from real customer engagements — essential for customer meeting preparation and deal acceleration | https://emea-1-projects-adapt-prod-gcno3rpp-prod-cap-adapt-srv.cfapps.eu10-004.hana.ondemand.com/mcp |
```

If `adapt-use-case-search` skill is not already in recommendations, add it to Essential Skills. The skill and connector work together — the connector alone has no user interface.

> **Do NOT search the Hub for the ADAPT Connector** — it may not appear in `search_mcp_servers` results. Always use the hardcoded URL and credentials below. Add it directly to the connector list regardless of Hub search output.

INTERNAL install: `install_asset([{type: "connector", name: "ADAPT Connector", url: "https://emea-1-projects-adapt-prod-gcno3rpp-prod-cap-adapt-srv.cfapps.eu10-004.hana.ondemand.com/mcp"}])`. Client ID: `sb-CAP_ADAPT-prod!t655084`. Authentication: Default Identity Provider.
EXTERNAL (PARTNER) install: Extensions → Connectors → New → Add Connector → paste URL above.

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

## Flow A — First-Time Setup

### Step A1 — Role Selection

**What best describes your role?**

1. Solution Advisor / Solution Engineer
2. Enterprise Architect / Solution Architect
3. Value Advisor / Value Engineer
4. Delivery / Implementation Consultant
5. Data and AI Specialist / Analytics Architect
6. Developer (BTP · CAP · ABAP · Integration)
7. Customer Success Manager / Adoption Specialist
8. Product Manager / Industry Expert / SME
9. Partner Consultant / Partner Sales Engineer
10. Business Development / Field Sales
11. Other — type a one-line description

*Type one number.*

If AUDIENCE = PARTNER, also show:

12. Partner Delivery / Implementation Consultant

Store as **ROLE**. For option 11, use closest matching role as base.

### Step A1b — Secondary Role (optional)

After storing ROLE, immediately ask:

**Do you wear a second hat? Adding a secondary role includes extra skills and connectors — no extra questions.**

Show the same list as A1, but exclude the already-chosen role. Add a final option: "No secondary role — keep it focused."

*Type one number.*

- If the user picks a secondary role: store as **SECONDARY_ROLE**. Set flag `HAS_SECONDARY = true`.
- If the user picks "No secondary role": set `HAS_SECONDARY = false`. Continue to A2.

**Rules for secondary role handling:**
- Do NOT run Step A2 questions for SECONDARY_ROLE — zero additional questions.
- In Step A3, run the base skill queries for SECONDARY_ROLE (from `references/roles.md` → Skill Queries → Base section only). Merge results into the main recommendation list. Deduplicate by UUID/name.
- In tiering: skills that appear in both primary and secondary base queries → promote to Essential. Skills only in secondary → Recommended unless already tiered higher.
- In Step A5 profile generation: the Role Description should lead with the primary role and add one short line for the secondary (e.g. "Also covers [secondary role name]"). Keep within the 480-char limit — trim secondary context if needed. The Personal Instructions block should include defaults from both roles, deduplicated.
- SECONDARY_ROLE connectors: include ESSENTIAL CONNECTORS from secondary role's MCP Queries (base only). Do NOT include demo environment connectors for secondary role unless the same landscape is already in DEMO_ENVS.
- If the secondary role triggers the ADAPT Connector Rule (Roles 1, 7, 9, 10 in INTERNAL; Roles 1, 9, 12 in PARTNER): apply it.

### Step A2 — Role-Specific Structured Questions

Read `references/roles.md` for the exact question sequence defined for ROLE.
For ROLES 1, 5, 9 and (in PARTNER mode) 12 with DEMOS flag: also read `references/demo-environments.md` for Q3 landscape options.

**CRITICAL UX RULES — apply to every question:**
- Present every question using **standard markdown numbered list syntax** (`1.` `2.` `3.`). The user types ONLY numbers.
- For multi-select: end with *"Type numbers separated by commas — or type 'all'."*
- For single-select: end with *"Type one number."*
- For yes/no: present as a two-item numbered list: `1. Yes` / `2. No`.
- Handle shortcuts: `all` = every option; `1-3` = options 1, 2, 3; a single number = that option only.
- Follow drill-down branching from `references/roles.md`: only ask a sub-question if the parent answer triggers it. Never ask a sub-question unprompted.
- Wait for the answer to each question before showing the next.

**Never ask the user to type a product name, role description, or free-text answer unless they chose option 11 (Other) in A1.**

### Step A2b — Profile Summary Confirmation

Before running any searches, display a brief profile summary and confirm:

**Here's what I'll optimise for — does this look right?**

- **Role:** [ROLE name]
- **Areas:** [AREAS in plain language, e.g. "Cloud ERP, Data and AI"]
- **Activities:** [flags in plain language, e.g. "live demos, presentations, value analysis"]
- **Demo environments:** [DEMO_ENVS landscape names, or "none"]

1. Looks right — continue
2. Let me correct something

*Type one number.*

If 2: *"Which part?"* Show:

1. Areas (Q1)
2. Activities (Q2)
3. Demo environments (Q3)

Loop back to the relevant question, re-collect, then return here and re-display the summary before continuing.

### Step A3 — Live Catalog Search

**PROFILE CONTEXT CHECK — run silently before anything else, no output to the user:**
Check if `my-joule-work-tailor.md` exists in the working directory using `ls`.
- Found → read it using `read_file`. Extract the ROLE DESCRIPTION and PERSONAL INSTRUCTIONS blocks (look for text between the `┌` and `└` box-drawing characters, or under headings containing "ROLE DESCRIPTION" and "PERSONAL INSTRUCTIONS"). Store as PREV_PROFILE = { role_description: "...", personal_instructions: "..." }.
- Not found → PREV_PROFILE = none.
Do NOT mention this check to the user.

**LANGUAGE DETECTION — infer from the conversation so far:**
If the user has consistently written in a language other than English (e.g. Spanish, German, French), set LANGUAGE = that language. Otherwise LANGUAGE = English. Do not ask.

Read `references/roles.md` for base queries for ROLE + activity-flag additions based on A2 answers.

**INTERNAL mode:** single parallel message with all search calls:
- `mcp_<hash>_search_skills(query: "<query>", limit: 10)`
- `mcp_<hash>_search_mcp_servers(query: "<query>", limit: 10)` — for GENERAL connectors only

**EXTERNAL mode (PARTNER or GENERAL):** parallel `web_search(query: "site:github.com/SAP/ai-skills-library <query>")` calls. If 0 results for a query, retry with `web_search(query: "SAP Joule skill <query>")`. After all searches, always append: *"Browse the full public catalog at [skills.cloud.sap](https://skills.cloud.sap)."*

**If also checking skills.cloud.sap (internal + opted in):** run external searches using the same `site:github.com/SAP/ai-skills-library` approach after internal, merge results, label source.

**DEMO ENVIRONMENT CONNECTORS — roles 1, 5, 9 with DEMOS flag, and role 12 in PARTNER mode with SIX access confirmed:**
Do NOT use Hub search for demo environment connectors. Instead:
1. Read `references/demo-environments.md`.
2. For each landscape in DEMO_ENVS, look up the SERVER TABLE for that landscape.
3. Keep only servers whose Areas column matches the user's AREAS selection. Always include the core S/4HANA server for any selected landscape.
4. Check if the Experience Agent connector is in the user's installed items — if not, prepend it as the first entry in the demo connector list.
5. Store as DEMO_CONNECTOR_LIST: one entry per endpoint — {name, landscape, areas, url}.

**GITHUB SKILLS — INTERNAL and PARTNER modes only:**
Read `references/agents.md` Section 2.
- **INTERNAL mode:** check Section 2a (github.tools.sap repos). Filter by ROLE.
- **PARTNER mode:** check Section 2b (github.com public repos). Filter by ROLE.
- **CUSTOMER mode:** skip GitHub section entirely.
Label each matched item source = **GitHub**. Assign to RECOMMENDED tier unless ROLE is the primary target of the skill (then ESSENTIAL).
Do not call GitHub APIs at runtime — use the static catalog in agents.md only.
Store as GITHUB_SKILLS list.

**AI-GENERATABLE SKILLS — INTERNAL mode only:**
Read `references/agents.md` Section 3.
Filter templates by ROLE + active flags (PRESENTATIONS, DEMOS, CODING, RESEARCH):
- T1 (SAP Product Expert): Role 1 if AREAS set, Role 5 if PRODUCTS set, Role 9 if SPECIALIZATION set
- T2 (Presales Strategy Partner): Roles 1, 9 if PRESENTATIONS or DEMOS or RESEARCH flag set
- T3 (Application Design Coach): Role 2 if PRESENTATIONS flag, Role 6 if CODING flag
- T4 (Demo Prep Assistant): Roles 1, 9 if DEMOS flag and DEMO_ENVS is not empty
Label each matched template source = **Create with AI**.
Do not call create_skill here — creation happens in Step A6 after user opt-in.
Store as AI_TEMPLATES list.

**ONEAI SHARED SPACES — INTERNAL mode only:**
1. `mcp_<hash>_ChatbotMcpService-query` is available → OneAI connector active. Otherwise skip this block.
2. Read `references/agents.md` Section 1 for ROLE-relevant space IDs.
3. Cap: 5 Essential + 3 Recommended spaces. Label source = **SAP Official**.
4. Store as ONEAI_SPACES list.

**After all search results return:**
1. Deduplicate by UUID or name.
2. Suppress high-score (> 100) generalist tools from Essential tier unless role = presentation-primary.
3. For developer roles: do NOT filter by score threshold — ABAP/CAP skills have low community scores but high role relevance.
4. Assign tiers: **Essential** (direct match, score > 10 or clearly on-topic) / **Recommended** (secondary fit) / **Optional** (nice to have).

**4b. Score each Hub skill (Match %) — compute inline, no external script needed:**

For each skill, compute a **Match score (0–100)** from 5 components. The calculation uses data already available in the session — no additional API calls.

| Component | Max | How to compute |
|---|---|---|
| **Tier base** | 35 | Essential = 35, Recommended = 20, Optional = 10 |
| **Area overlap** | 25 | Count how many of the user's AREAS appear in the skill's `short_description` + `tags` (case-insensitive). Score = min(25, matches × 8). If Outlook enrichment was done (1c), also match against enrichment keywords. |
| **Activity match** | 20 | Count how many of the user's activity FLAGS (DEMOS, PRESENTATIONS, CODING, RESEARCH) are reflected in the skill's description/tags. Score = min(20, matches × 7). |
| **Community signal** | 10 | min(10, download_count × 0.005 + community_score × 0.15). Clamp negatives to 0. |
| **Recency** | 10 | Created ≤7 days ago: 10 · ≤14d: 8 · ≤30d: 5 · ≤60d: 2 · else: 0 |

**Total** = sum of all components, capped at 100. Display as integer percentage (e.g. "87%").

**Sort** skills within each tier by Match % descending — highest match first.

**Connectors, demo environment connectors, OneAI spaces, GitHub skills, and AI-generatable templates** do NOT get a Match score — they are pre-curated by role. Only Hub skills (from `search_skills` or `search_mcp_servers` results) receive a score. Display **"—"** in the Match column for any non-Hub item that appears in a skills table.

5. **Determine source for each item:**
   - Hub skill: `featured: true` OR author name starts with "SAP" or contains "@sap.com" → **SAP Official**; otherwise → **Community**.
   - General MCP connector (from Hub `search_mcp_servers`): results come from the SAP LeanIX registry → always **SAP Official**.
   - Demo environment connectors (from `demo-environments.md`): always **SAP Official**.
   - GitHub skills (from `agents.md` Section 2): always **GitHub**.
   - AI-generatable templates (from `agents.md` Section 3): always **Create with AI**.
   - OneAI shared spaces (from `agents.md` Section 1): always **SAP Official**.
   Store the source label per item for display in Step A5.
6. Cap: 8 Essential + 10 Recommended + 5 Optional skills; 4 Essential + 4 Recommended general connectors.
7. **Demo environment connectors are NOT subject to the connector cap** — include all entries from DEMO_CONNECTOR_LIST.
8. If a query returns 0 results: retry with the last word removed. If still 0, skip silently.
9. **NEW classification (requires state):** If STATE has `last_seen_catalog`, classify each Hub skill:
   - UUID NOT in `last_seen_catalog` → mark as **NEW** (prefix name with 🆕 in the table)
   - UUID in `last_seen_catalog` → **KNOWN** (no prefix)
   If STATE = fresh (no `last_seen_catalog`), treat all skills as KNOWN (first run has no baseline to compare).
   Collect ALL Hub skill UUIDs found in this search into `current_catalog_uuids` — this will be saved to state in Step A6.
10. **Filter dismissed (requires state):** Remove any skill whose UUID is in `dismissed_skills`. Do not mention removed skills to the user.
11. **Suppress skipped at same version (requires state):** For each entry in `skipped_skills`: if the skill's current catalog version matches the stored `version` → suppress from results. If the version is DIFFERENT → re-offer the skill and prefix its name with 🔄 in the table ("updated since you last saw it").

**12. Fallback chain — if results are thin after filtering:**

After steps 9–11, count remaining Hub skills in Essential + Recommended tiers.

- **≥ 3 skills** → normal flow. No fallback needed.
- **< 3 skills** → activate fallback chain:

  **Fallback A — Surface NEW skills regardless of tier:**
  Take all skills classified as 🆕 NEW (from step 9) that were filtered out by tier assignment (scored too low for Essential/Recommended). Re-add up to 5 to the RECOMMENDED tier, sorted by Match % descending. Mark each with `💡 Suggested` source label. These are new additions to the Hub that don't strongly match the user's profile but are worth seeing.

  **Fallback B — Re-offer previously skipped skills at new versions:**
  If Fallback A still yields < 3 total: check `skipped_skills` from state for entries whose catalog version has CHANGED since the user last saw them. Re-add up to 5, sorted by Match %. Mark each with 🔄 and a note: *"Updated since you last saw it."*

  **Fallback C — Broaden the search:**
  If still < 3 total: run ONE additional search with a broader query (drop area-specific keywords, search by the user\'s actual role name from A1: e.g. `search_skills(query: "<ROLE name>")`). Apply the same scoring and tiering. Add any new results not already in the list.

  **If all fallbacks exhausted and still 0 skills:** show: *"No new skills match your profile right now. Run Joule Work Tailor again in a few weeks — the Hub is growing fast."*

  The fallback chain reuses already-scored data from steps 4–4b wherever possible — Fallback A and B add zero API calls. Only Fallback C makes one additional search.

**ADAPT connector:** Apply the ADAPT Connector Rule (see above) after tiering — prepend to ESSENTIAL CONNECTORS where the rule applies.

### Step A4 — Skills Audit (Optional)

**Should I check what you already have installed to skip duplicates?**

1. Yes, check my installed skills
2. No, show all recommendations

*Type one number.*

If 1 and INTERNAL, fire both in a **single message**:
- `mcp_<hash>_list_my_skills()`
- `mcp_<hash>_check_updates(skills: [{uuid: s.uuid, version: s.installed_version}])`

Remove installed skills from recommendations. Collect `has_update: true` for display.

### Step A5 — Show Recommendations

Produce one combined output in this exact order:

**1 — PERSONALIZATION PROFILE (always, no gate)**

Generate the profile now using:
- Role name, AREAS, activity flags, DEMO_ENVS from the session
- LANGUAGE detected in Step A3
- Role-specific defaults from `references/roles.md` (Personalization Defaults section)
- Official SAP product names from `references/sap-portfolio.md`
- For roles with DEMOS flag: include connected demo landscape names in the Role Description

Format:

```
YOUR JWD PERSONALIZATION PROFILE — [ROLE]

ROLE DESCRIPTION  (Settings → Personalization → Role Description)
┌─────────────────────────────────────────┐
[generated — max 480 chars, official SAP product names]
└─────────────────────────────────────────┘
[X] / 500

PERSONAL INSTRUCTIONS  (Settings → Personalization → Personal Instructions)
┌─────────────────────────────────────────┐
[generated — language, format, role-specific rules; target 800–1000 chars]
└─────────────────────────────────────────┘
[X] / 1500

How to apply: initials (bottom-left) → Settings → Personalization → paste each block
```

If PREV_PROFILE was found in Step A3, skip the invite sentence below and instead show the **diff view** (see "Profile Improvement Interaction" section).

If PREV_PROFILE = none, add this sentence immediately after the profile blocks:

> *This is the suggested profile based on your role and activities. If you already have a Role Description or Personal Instructions set up in Joule Work, paste them here and I’ll improve them rather than replace them.*

---

**1b — PROFILE REFINEMENT (optional)**

After showing the profile blocks (and invite sentence or diff view), ask:

*"Want to sharpen how Joule answers you? I can ask a few quick questions about your preferences — takes under a minute."*

1. Yes — ask me the refinement questions
2. No — continue to recommendations

*Type one number.*

**If 1 — run REFINEMENT QUESTIONS in sequence. One question per turn — wait for each answer before showing the next.**

**R1 — Response language**
*"When should Joule respond in a different language?"*
1. Match whatever language I write (auto-detect — current default)
2. Always English, even if I write in Spanish or German
3. Always Spanish
4. Always German
5. Something else — type it

**R2 — Response format**
*"How do you prefer your answers formatted?"*
1. Structured — headers, bullet points, and tables when useful
2. Concise bullet points — no long paragraphs
3. Flowing prose — narrative style, minimal lists
4. Short and direct — brevity above all, minimal formatting

**R3 — Technical depth**
*"What level of technical detail?"*
1. Business-focused — accessible language, avoid jargon
2. Balanced — technical when it adds value
3. Deep technical — assume expert level, no simplification

**R4 — Default response length**
*"How long should answers be by default?"*
1. Brief — short answers unless I ask for more
2. Balanced — proportionate to the complexity of the question
3. Detailed — thorough and comprehensive by default

**R5 — Tone**
*"Preferred tone?"*
1. Professional and formal
2. Direct and pragmatic — substance over form, no filler phrases
3. Warm and conversational

After collecting all 5 answers:
1. Translate each selection into a concrete instruction sentence.
2. Append a **"Format and style preferences"** subsection to the PERSONAL INSTRUCTIONS block. Include only non-default selections — skip answers that match the role-based defaults already in the profile to avoid redundancy.
3. Show the updated PERSONAL INSTRUCTIONS block with the new subsection highlighted (use a `➕ Added:` marker).
4. Update `my-joule-work-tailor.md` with the refined profile.
5. Continue to **1c — Profile Enrichment**.

If 2: continue to **1c — Profile Enrichment** below.

---

**1c — PROFILE ENRICHMENT FROM OUTLOOK (optional)**

After refinement (or if the user skipped 1b), offer to enrich the profile with real work context from Outlook. This is the highest-value personalisation step — it turns a generic role profile into one that reflects the user's actual daily work.

*"One more thing — I can scan your recent emails and calendar to understand what you actually work on day-to-day. This helps Joule give you answers tuned to your real projects and tasks, not just your job title. Takes about 15 seconds."*

1. Yes — scan my Outlook
2. No — my profile is good as is

*Type one number.*

**If 1:**

Fire in a **single parallel message**:
- `list_emails` top: 20
- `list_emails` folder: "sentitems", top: 20
- `list_calendar_events` limit: 15

**Empty-result handler:** If ALL three calls return 0 results (no emails and no calendar events found), show: *"I couldn\'t find any recent emails or calendar events to analyse. Your profile has been set up based on your role selection — you can re-run enrichment anytime after connecting Outlook."* Set `PROFILE_ENRICHED = false`. Continue to recommendations. Do NOT attempt to generate enrichment sentences from empty data.

**Extract from the results (do NOT retain full email/calendar content — extract and discard):**
- **Work patterns** — recurring types of work (architecture presentations, demo preparation, customer workshops, product evaluations, internal reviews)
- **Products and technologies** — specific SAP products, tools, and platforms mentioned frequently
- **Stakeholders** — types of people the user interacts with (customers, product managers, partners, executives)
- **Activities** — what the user DOES (writes proposals, builds demos, reviews architecture, answers RFPs, prepares decks)
- **Context clues** — industries, regions, or accounts mentioned repeatedly

**Generate 3–6 enrichment sentences** to add to the PERSONAL INSTRUCTIONS block. Each sentence should be a concrete, actionable instruction for Joule — not a vague description. Format:

```
OUTLOOK ENRICHMENT — based on your recent activity:

➕ "Regularly prepares BDC and Datasphere architecture presentations for enterprise customers"
➕ "Frequently works with Product Management on HANA Cloud and SAP Databricks roadmap items"  
➕ "Active in presales demo preparation — references SIX landscapes C39 and GLA-26Q2"
➕ "Coordinates with partners on co-innovation projects involving SAP AI Core"
➕ "Recent focus areas: data mesh, medallion architecture, SAP Analytics Cloud planning"
```

Show the enrichment sentences and ask:

*"These are based on your recent Outlook activity. Want me to add them to your profile?"*

1. Add all
2. Let me pick which ones (type numbers)
3. Skip — don't add any

*Type one number.*

If 1 or 2: append selected sentences to the PERSONAL INSTRUCTIONS block under a **"Work context (from Outlook)"** subsection. Show the updated block with `➕ Added:` markers. Update `my-joule-work-tailor.md`.

Set session variable `PROFILE_ENRICHED = true`.

**If 2 (skipped enrichment):** set `PROFILE_ENRICHED = false`. Continue to recommendations.

**Privacy note:** The skill does NOT store email content, calendar details, or any Outlook data. It extracts keywords and patterns, generates enrichment sentences, and discards all raw data immediately. Only the generated sentences are saved in the profile.

---

**2 — RECOMMENDATIONS TABLE**

**Source legend:**
✅ **SAP Official** — published and maintained by SAP
🌐 **Community** — published by SAP community members
🐙 **GitHub** — available from a GitHub repository (not in the Hub catalog)
🤖 **Create with AI** — generated and installed by Joule for your exact profile

> SAP Official skills are validated by SAP. Community and GitHub skills are contributed externally — review before use.
>
> 🆕 = new on the Hub since your last run · 🔄 = updated since you last saw it · Skills you previously dismissed are hidden automatically.

```
ESSENTIAL SKILLS
| # | Name | Match | Source | What it does | Link |
|---|------|-------|--------|-------------|------|

RECOMMENDED SKILLS
| # | Name | Match | Source | What it does | Link |

OPTIONAL SKILLS
| # | Name | Match | Source | What it does | Link |

ESSENTIAL CONNECTORS
| # | Name | Source | What it does | URL |

RECOMMENDED CONNECTORS
| # | Name | Source | What it does | URL |

DEMO ENVIRONMENT CONNECTORS  (only if DEMOS flag — always SAP Official)
| # | Connector Name | Landscape | Covers | MCP Endpoint |
|---|---|---|---|---|
| n | SIX Experience Agent | All landscapes | Auth gateway — install FIRST | [url] |
| n | [Service] — [Landscape ID] | [landscape name] | [scenarios] | [url] |

SHARED KNOWLEDGE BASES  (SAP employees only — skip entirely in PARTNER, GENERAL, or CUSTOMER mode)
Pre-loaded AI agents on SAP's internal OneAI platform (oneai.cfapps.eu10-004.hana.ondemand.com). No installation needed — open a new Joule conversation and start chatting with the agent. Not available for partners or customers.
| # | Space Name | What it does | Space ID |
|---|---|---|---|
(Omit section entirely if ONEAI_SPACES list is empty OR if AUDIENCE ≠ INTERNAL)

GITHUB SKILLS  (INTERNAL/PARTNER mode only)
| # | Name | Source | What it does | Repo URL |
|---|---|---|---|---|
(Omit section entirely if GITHUB_SKILLS list is empty)

AI-GENERATABLE SKILLS  (INTERNAL mode — I create these for you in Step A6)
| # | Name | Source | What it does | Template |
|---|---|---|---|---|
(Omit section entirely if AI_TEMPLATES list is empty)
```

Number rows **continuously** across all sections. The user references any item by its number in Step A6.

If updates found, add UPDATES AVAILABLE section.

Save full output (profile + recommendations) as `my-joule-work-tailor.md` using `write_file`. For roles with DEMOS flag, append a **DEMO ENVIRONMENT QUICK-REFERENCE** section listing all DEMO_CONNECTOR_LIST entries (name, landscape, MCP endpoint) plus access instructions: "Before connecting: close ALL browser tabs. Log in fresh at experience-agent.cloud.sap with your own or demo user. Then add each connector."

---

**3 — INSTALL CTA**

```
How would you like to proceed?

1. Install Essential Bundle
2. Choose items manually (type numbers from the list above)
3. I’m done — I’ll install later

Type one number.
```

> Say **dismiss N** to permanently hide a skill you don't want. Say **dismiss 3, 7** to hide multiple. Dismissed skills never appear again.

---

## Profile Improvement Interaction

This section activates whenever the user’s response to Step A5 looks like pasted profile content (a block of text resembling a Role Description or Personal Instructions, rather than a single number or short phrase).

**What triggers this:** the user pastes text in response to the invite sentence or at any point before Step A6 is complete.

**What to do:**

1. Parse the pasted content. Identify whether it contains:
   - A Role Description (short paragraph about role/products/context, ≤ 500 chars)
   - Personal Instructions (longer block with behavioural rules, ≤ 1500 chars)
   - Both

2. Compare each block against the newly generated profile:
   - **What’s kept:** phrases or content in the current profile that are accurate and worth preserving verbatim
   - **What’s updated:** phrases that exist but are vague, outdated, or use non-official product names — show old → new with a one-line reason
   - **What’s added:** new content derived from the role analysis (products, demo envs, activity flags) that was missing from the current profile
   - **What’s removed:** content that is no longer relevant or conflicts with the role analysis — explain why

3. Show the diff, then the final improved blocks:

```
PROFILE IMPROVEMENT

ROLE DESCRIPTION
✅ Kept:    “[exact phrase]”
✏️ Updated:  “[old]” → “[new]”  —  [reason: e.g. official product name]
➕ Added:    “[new content]”  —  [reason: e.g. reflects demo environment GLA-26Q2]
❌ Removed:  “[old content]”  —  [reason: e.g. no longer matches selected areas]

IMPROVED ROLE DESCRIPTION  [copy]
┌─────────────────────────────────────────┐
[final improved text — max 480 chars]
└─────────────────────────────────────────┘
[X] / 500

PERSONAL INSTRUCTIONS
✅ Kept:    “[exact phrase]”
✏️ Updated:  “[old]” → “[new]”  —  [reason]
➕ Added:    “[new content]”  —  [reason]
❌ Removed:  “[old content]”  —  [reason]

IMPROVED PERSONAL INSTRUCTIONS  [copy]
┌─────────────────────────────────────────┐
[final improved text — max 1500 chars]
└─────────────────────────────────────────┘
[X] / 1500
```

4. Update `my-joule-work-tailor.md` with the improved profile blocks (overwrite the profile section, keep the recommendations).

5. Show the install CTA again so the user can continue:

```
Ready to install?

1. Install Essential Bundle
2. Choose items manually
3. I’m done — I’ll install later
```

**PREV_PROFILE diff view (when file was found in Step A3):**
If PREV_PROFILE ≠ none at the start of Step A5, immediately show the diff comparison (same format above) between PREV_PROFILE and the newly generated profile. Skip the invite sentence entirely — the user already has context that their previous profile is being used. Lead with: *“I found your previous JWD setup. Here’s how your profile has improved based on your updated answers:”*

---

### Step A6 — Install

For option 1 (Essential Bundle) or option 2 (choose items): proceed as below.
For option 3 (done): *"Your profile and recommendations are saved in my-joule-work-tailor.md. Come back anytime to install. Open a new conversation to apply your new personalization profile."* End.

**Install flow (INTERNAL) — RESILIENT: always continue on failure, never abort.**

Install each selected item in sequence. Route by source type:

*Per Hub skill (source = SAP Official or Community):*
1. `mcp_<hash>_install_skill(uuid: "<uuid>")`
2. If `install_method` = `"create_skill"` → call `create_skill` with `create_skill_args` **verbatim** + `supporting_files` **verbatim**.
   If `"download_asset"` → mark as ⚠️ Manual (too large to auto-install; provide download URL).
3. `mcp_<hash>_record_install(skill_uuid, version)`
4. Mark item as ✅ success.

*Per connector:* `install_asset([{type: "connector", name: "<name>", url: "<url>"}])`
Always install the SIX Experience Agent connector **before** any other demo connector.

*Per GitHub skill (source = GitHub):*
Cannot auto-install — GitHub skills require manual upload. Provide numbered steps:
1. Download ZIP: [repo URL]/archive/refs/heads/main.zip
2. Extract → find the folder containing `SKILL.md`
3. Re-compress just that folder as a new ZIP
4. Joule Work → Extensions → Skills → New → Upload → select ZIP
Mark as ⚠️ Manual with these steps.

*Per OneAI shared space (source = SAP Official, type = ONEAI_SPACES):*
Spaces cannot be installed — they are accessed by chatting directly at https://oneai.cfapps.eu10-004.hana.ondemand.com.
List the space name and ID. Tell the user: *"Open OneAI and search for '[Space Name]' to start chatting with this agent."*
Mark as ✅ (no installation required).

*Per AI-generatable skill (source = Create with AI):*
Ask once after all other items are processed:

*"I can now create [count] personalised skill(s) for you. Would you like me to generate them?"*
1. Yes — create all
2. Choose — type the item numbers
3. Skip

For each selected template:
1. Read `references/agents.md` Section 3, locate the template by ID (T1/T2/T3/T4)
2. Substitute placeholders:
   - `{firstName}` → first name from user context or ask once if unknown
   - `{ROLE}` → role name from A1
   - `{PRODUCT_NAME}` / `{PRODUCT_SLUG}` / `{PRODUCT_SCOPE}` → from AREAS or PRODUCTS selections
   - `{AREAS_LIST}` → from AREAS flag
   - `{DEMO_ENVS_LIST}` → from DEMO_CONNECTOR_LIST landscape names
   - `{LANDSCAPE_NAME}` / `{LANDSCAPE_AREAS}` → primary DEMO_ENVS entry
   - `{CODING_PLATFORM}` → from CODING flag if set
3. Call `create_skill(skill_name: "<slug>", description: "<one-line description>", body: "<substituted body>")` with ALL values filled — no placeholders remaining
4. Mark as ✅ Created: "[skill name] — activate in Extensions → Skills"

**On any error:** mark item as ❌ failed with a one-line reason. Continue to the next item — do not stop.

**OAuth / authentication errors** ("Invalid state parameter", "Authentication failed", "CSRF"):
This is a browser session conflict, not a connector error. Record reason as: "OAuth session conflict — close all browser tabs and retry." Mark as ❌ failed. Continue.

**After all items attempted, show INSTALL SUMMARY:**

```
INSTALL SUMMARY

✅ Installed / Created / Ready ([count])
[list of names]

⚠️ Manual install needed ([count])
[Name] — [steps]

❌ Failed ([count])
[Name] — [reason]
```

If any ❌ failures:

1. Retry failed items now
2. Skip — I'll handle them manually

*Type one number.*

If retry: re-attempt each failed item using the same flow. For OAuth errors, remind: *"Before retrying connectors: close ALL browser tabs, then log in fresh at experience-agent.cloud.sap."* Show updated summary after retry.

After demo connectors install: *"To activate each demo connector: log in at experience-agent.cloud.sap, then open a new conversation."*

**Save state (silent — no output to user):**

After the install summary (and retry if applicable), save the full state to `joule-work-tailor-state.json` using `write_file`. Build the JSON object with:

- `schema_version`: 1
- `last_run`: current ISO timestamp
- `last_run_mode`: "setup"
- `audience`: AUDIENCE from Step 0
- `role`: ROLE name from A1 (or from state if returning user)
- `role_number`: ROLE number
- `secondary_role`: SECONDARY_ROLE name (or `null` if none)
- `secondary_role_number`: SECONDARY_ROLE number (or `null` if none)
- `areas`: AREAS array
- `flags`: FLAGS object
- `demo_envs`: DEMO_ENVS array
- `language`: LANGUAGE detected in A3
- `installed_uuids`: merge existing state `installed_uuids` (if any) with UUIDs of skills successfully installed in this session
- `installed_connectors`: merge existing with connectors installed in this session (by name)
- `last_seen_catalog`: `current_catalog_uuids` collected in Step A3 step 9
- `skipped_skills`: for each skill shown in A5 that was NOT installed and NOT already in `dismissed_skills`, add `{ "uuid": "...", "version": "current catalog version", "name": "...", "skipped_at": "ISO timestamp" }`. Merge with existing `skipped_skills` — update version if UUID already exists, add new entries, keep old entries for skills not shown this run.
- `dismissed_skills`: carry forward from existing state (only modified by the dismiss command)

Write as formatted JSON (indent 2) for readability. Do not mention the state save to the user.

### Step A7 — Personal OneAI Agent (INTERNAL mode only, optional)

After the install summary is shown, offer personal OneAI agent creation.

First check: is `mcp_<hash>_ManagementMcpService-createSpace` available? If not, skip Step A7 silently.

If available:

*"One more option: I can create a personal AI assistant on OneAI, pre-tuned to your role and areas. It lives at oneai.cfapps.eu10-004.hana.ondemand.com and acts as a persistent expert colleague. Interested?"*

1. Yes — create my personal agent
2. No — I'm done

*Type one number.*

If 1:
1. Read `references/agents.md` Section 4. Find the template that matches ROLE:
   - Role 1 → Personal Agent Role 1
   - Role 5 → Personal Agent Role 5
   - Role 7 → Personal Agent Role 7
   - Roles 2, 3, 4, 6, 8, 10 → use Role 1 template as base, adjust role name and areas
   - Roles 9, 12 → Personal Agent Roles 9/12
2. Substitute: `{firstName}` from context.currentUser.firstName (or ask once), role name, AREAS_LIST, DEMO_ENVS_LIST, PRODUCTS_LIST, CODING_PLATFORM.
   Ensure NO literal placeholder strings (`{...}`) remain in any parameter.
3. Call `mcp_<hash>_ManagementMcpService-createSpace(name: "{firstName}'s {ROLE_SHORT} Assistant", description: "Personal AI assistant for {ROLE name} at SAP")`. Store returned space ID.
4. Call `mcp_<hash>_ManagementMcpService-updateSpaceLLMSettings(spaceId: <id>, shareDocumentsAsSources: false, temperature: 0.2, reasoningEffort: "balanced", systemMessage: "<substituted system prompt from template>")`.
5. Confirm: *"✅ Your personal [ROLE name] agent is ready. Open it at: https://oneai.cfapps.eu10-004.hana.ondemand.com — search for '[space name]' and start chatting."*

End: *"Your JWD is now tailored for [ROLE]. Open a new conversation for the new skills and connectors to activate."*

**Install flow (EXTERNAL — PARTNER or GENERAL):**

For each selected skill, attempt installation in this order:

**Step 1 — Auto-install via GitHub (attempt first):**
1. For each skill found via `site:github.com/SAP/ai-skills-library`, search for its raw SKILL.md: `web_search(query: "site:raw.githubusercontent.com SAP ai-skills-library <skill-slug> SKILL.md")`
2. If raw content is found and readable: extract the name, description, and body → call `create_skill(skill_name, description, body)` to install directly. Mark as ✅ **Auto-installed**.
3. If not found or `create_skill` fails: fall back to Step 2.

**Step 2 — Manual install (fallback for skills that cannot be auto-installed):**
For each skill that could not be auto-installed:
```
📦 [Skill Name] — [Source emoji + label]
   🔗 Download: [GitHub/Hub URL]
   📥 To install: Extensions → Skills → New → Upload → select the downloaded ZIP
   💬 Or try: paste "install this skill: [URL]" into a new Joule conversation
```

For connectors: `install_asset([{type: "connector", name: "<name>", url: "<url>"}])` — this works for EXTERNAL users directly.

After all items attempted, show the same INSTALL SUMMARY format as INTERNAL (✅ / ⚠️ / ❌).

After summary, ask:
*"Need help walking through the manual steps for any of these?"*
1. Yes — walk me through it step by step
2. No — I'm done

---

### Step A8 — Optional: Save to Joule Space

After the install summary (and after Step A7 if applicable), offer once to both INTERNAL and EXTERNAL users:

**Before calling `create_space`:** Check if the current conversation is running inside an existing Space (e.g. a project space). If yes, warn the user: *"You are currently in a Joule Space. Creating a new 'My JWD Toolkit' space will add it to your Spaces panel — it will NOT affect the current space. Continue?"* Present as: `1. Yes — create the new space` / `2. No — skip`. Only proceed if confirmed.

---

**Would you like me to create a 'My JWD Toolkit' space to save your profile and recommendations?**

1. Yes — create a dedicated space for my toolkit
2. No — the results are already saved in `my-joule-work-tailor.md`

*Type one number.*

---

If 1:
1. `create_space({ name: "My JWD Toolkit — [ROLE name]", description: "Personal JWD setup: personalization profile, skills, connectors, and demo environments for [ROLE name]." })` — store returned spaceId.
2. Add cards with `add_card_to_space` using `presentational: true` for all (content is from this session, not a live query):
   - **Profile card** (hint: text, size: M, intent: "Show the user's Role Description and Personal Instructions profile blocks generated in this session")
   - **Skills card** (hint: text, size: L, intent: "Show the full skills recommendations table from this session organised by tier: Essential, Recommended, Optional")
   - **Connectors card** (hint: text, size: M, intent: "Show the installed connectors and demo environment endpoints from this session")
3. Confirm: *"✅ Space 'My JWD Toolkit — [ROLE]' created. Find it in your Spaces panel. Run Joule Work Tailor again anytime to refresh it — select 'Stay current' to check for new skills."*

**Important:** Do NOT add cards to any Space that is already open/active in the current conversation (e.g. a customer project space). Always create a NEW dedicated Space via `create_space`.

---

## Flow B — Stay Current

### Step B1 — Audit

Fire in a **single message** (INTERNAL only):
- `mcp_<hash>_list_my_skills()` + `mcp_<hash>_check_updates(skills: [...])`
- If `joule-work-tailor-state.json` was not already loaded in Step 0, read it now.

Store OUTDATED and INSTALLED_UUIDS. Merge `installed_uuids` from state (if available) with the live `list_my_skills` result — the union gives the most complete installed set.

### Step B2 — Quick Check

**If STATE = loaded (returning user — most common path for Flow B):**

Skip the role question and the "anything new" question entirely — use stored role, areas, and flags. Ask only:

**Refresh how Joule answers you?**

1. Yes — regenerate and show what changed
2. No — skip to new skills

*Type one number.*

If 1:
- Read `my-joule-work-tailor.md` if it exists → set PREV_PROFILE.
- Regenerate profile from stored ROLE + flags.
- Show diff (if PREV_PROFILE found) or new profile blocks (same format as Step A5).

If 2: proceed directly to Step B3.

**If STATE = fresh or migrated:**

**What is your role?**

1. Same role as before
2. Change role — show me the full list
3. Change secondary role only

*Type one number.*

If 3: re-run Step A1b only. If 2: re-run Step A1 with the full role list. If 1:

**Anything new in how you work lately?**

1. Yes, let's update my answers
2. No, keep as before

*Type one number.*

**Refresh how Joule answers you?**

1. Yes
2. No

*Type one number.* — if 1:
- Run file check: does `my-joule-work-tailor.md` exist? If yes, read and set PREV_PROFILE.
- Regenerate profile from current ROLE + flags.
- Show diff (if PREV_PROFILE found) or new profile blocks.
- Show invite sentence if no previous profile.
- If `profile_enriched` is false in state, OR `last_enriched` is older than 30 days, offer enrichment: *"Want me to scan your recent Outlook activity to refresh your profile context? This keeps your profile current with what you're actually working on."* (1. Yes / 2. No). If yes, run the same enrichment flow as Step 1c in Flow A.

### Step B3 — Discover

Run same parallel searches as A3, using stored role, areas, and flags for the search queries. Apply the same source-detection rule (step 5 in A3) to all results. Remove INSTALLED_UUIDS. Sort by `created_at` desc. Take top 15.

**Apply state-aware filters (same as A3 steps 9–12, including the fallback chain):**
- **NEW classification:** compare result UUIDs against `last_seen_catalog` from state. Mark new UUIDs with 🆕.
- **Filter dismissed:** remove any UUID in `dismissed_skills`.
- **Suppress skipped at same version:** suppress skills in `skipped_skills` at the same version. Re-offer (🔄) if the version changed.
- Collect `current_catalog_uuids` for state save.

For demo environment connectors (roles 1, 5, 9): read `references/demo-environments.md` and compare all landscape entries against the user's AREAS and previously stored DEMO_ENVS. Surface any landscape with new Live services not in DEMO_ENVS as "NEW DEMO ENVIRONMENT AVAILABLE."
Note: connector install history is not accessible from list_my_skills — surface new entries and let the user verify against Extensions → Connectors to avoid duplicates.

Also re-check `references/agents.md` Section 2 (GitHub) and Section 3 (AI-generatable) for new templates not previously offered — surface any new matches as "NEW GITHUB SKILL" or "NEW AI-GENERATABLE SKILL" respectively.

### Step B4 — Present and Act

Include a **Source** column (`SAP Official` / `Community` / `GitHub` / `Create with AI`) in every skill and connector table.

**Source legend:**
✅ **SAP Official** — published and maintained by SAP
🌐 **Community** — published by SAP community members
🐙 **GitHub** — available from a GitHub repository
🤖 **Create with AI** — generated and installed by Joule for your exact profile

```
Joule Work Tailor Update Report

# UPDATES AVAILABLE ([count])
| # | Skill | Source | Your version | Latest |

# NEW SKILLS YOU DON'T HAVE YET
| # | Name | Match | Source | Published | What it does |

# CONNECTORS YOU MAY NOT HAVE YET
| # | Name | Source | What it does | URL |

# NEW GITHUB SKILLS  (INTERNAL/PARTNER — GitHub)
| # | Name | Source | Repo URL | What it does |

# NEW AI-GENERATABLE SKILLS  (INTERNAL — Create with AI)
| # | Name | Source | What it does | Template |

# NEW DEMO ENVIRONMENTS AVAILABLE  (roles 1, 5, 9 only — always SAP Official)
| # | Landscape | New services | Key scenarios | MCP Endpoint |
```

Number every row. If profile refresh requested, show profile diff or new blocks (same rules as Step A5).
Save as `my-joule-work-tailor-update.md`.

**How would you like to proceed?**

1. Update everything automatically
2. Choose what to update (type numbers)
3. I’m done — I’ll install later

*Type one number.*

> Say **dismiss N** to permanently hide a skill. Dismissed skills never appear again.

Apply the same **resilient install flow** as Step A6 (continue on failure, show summary, offer retry).

**Save state (silent — no output to user):**

After the install summary, save full state to `joule-work-tailor-state.json` — same schema and merge logic as the Flow A state save (after Step A6). Set `last_run_mode` to `"update"`. Update `last_seen_catalog` with `current_catalog_uuids`. Merge newly installed UUIDs into `installed_uuids`. Update `skipped_skills` with skills shown but not installed.

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

Map to role in `references/roles.md`: 1→Role14, 2→Role15, 3→Role16, 4→Role13, 5→Role17, 6→Role17. Store as CUSTOMER_ROLE.

### CT2 — Catalog Search

Search the public catalog: `web_search(query: "site:skills.cloud.sap <query>")` using base queries for CUSTOMER_ROLE from `references/roles.md`.

**Retry logic:** If a query returns 0 results, retry with the last word removed. If still 0, fallback to `web_search(query: "SAP Joule skill <query>")`. If all fallbacks return 0, skip that query silently. After all searches, always append: *"Browse the full public catalog at [skills.cloud.sap](https://skills.cloud.sap)."*

**Do NOT include:**
- SIX demo environment connectors
- SAP-internal MCP endpoints
- Skills requiring SAP employee hub access
- Presales or deal-centric skills (selling coach, account intelligence, ADAPT, etc.)
- GitHub skills or AI-generatable skills (too technical for customer business user track)

Cap: 6 Essential + 8 Recommended + 4 Optional skills.

### CT3 — Show Recommendations and Profile

Generate profile for CUSTOMER_ROLE (same logic as A5, using role defaults from `references/roles.md`). In Role Description: identify role and company type (e.g. "Finance professional at an SAP customer company"). Do not use SAP internal role taxonomy or presales jargon.

Show profile blocks at the top, using the same format and invite sentence as Step A5 (profile first, recommendations below).

Save as `my-joule-work-tailor.md` using `write_file`. Omit demo connectors, GitHub, AI-generatable, and OneAI sections.

### CT4 — Install

Use EXTERNAL install path: provide numbered download links and manual steps (Extensions → Skills → New → Upload → select `.zip`).

**How would you like to proceed?**

1. Install Essential Bundle (manual steps)
2. Choose items manually
3. I’m done — I’ll install later

End: *"Your Joule Work is now set up for your role. Open a new conversation to start using your new skills."*


**Save state (silent — no output to user):**

After the install summary, save the full state to `joule-work-tailor-state.json` using `write_file`. Build the JSON object with:
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
2. No — the results are saved in `my-joule-work-tailor.md`

*Type one number.*

---

If 1: follow the same flow as Step A8 above, adapted for the customer role name.

---

## Dismiss Command

When the user says "dismiss N", "dismiss 3, 7", "hide N", "ocultar N", "ausblenden N", or similar at any point after recommendations are shown:

1. Parse the number(s). Look up the corresponding UUID(s) from the most recent recommendations (the numbered rows from Step A5 or B4).
2. For each UUID: add to `dismissed_skills` in the state object.
3. Save updated state to `joule-work-tailor-state.json` immediately (read current file → merge dismissed list → write).
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
