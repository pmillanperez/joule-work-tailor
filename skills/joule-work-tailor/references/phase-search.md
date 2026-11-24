# Phase: Search & Scoring (Step A3)

This file is loaded when intake is complete and search begins. Read it with `read_file` at the start of Step A3.

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

### Step A3 — Live Catalog Search

**PROFILE CONTEXT CHECK — run silently before anything else, no output to the user:**
Check if `joule-work-tailor/my-joule-work-tailor.md` exists using `ls` on the `joule-work-tailor/` directory.
- Found → read it using `read_file`. Extract the ROLE DESCRIPTION and PERSONAL INSTRUCTIONS blocks (look for text between the `┌` and `└` box-drawing characters, or under headings containing "ROLE DESCRIPTION" and "PERSONAL INSTRUCTIONS"). Store as PREV_PROFILE = { role_description: "...", personal_instructions: "..." }.
- Not found → check legacy path `my-joule-work-tailor.md` in the root working directory. If found there, read and extract same blocks. Set PREV_PROFILE accordingly.
- Neither found → PREV_PROFILE = none.
Do NOT mention this check to the user.

**LANGUAGE DETECTION — infer from the conversation so far:**
If the user has consistently written in a language other than English (e.g. Spanish, German, French), set LANGUAGE = that language. Otherwise LANGUAGE = English. Do not ask.

The role file for ROLE is already in context — use it directly for base queries and activity-flag additions based on A2 answers.

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

