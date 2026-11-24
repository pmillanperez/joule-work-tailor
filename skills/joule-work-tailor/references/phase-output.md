# Phase: Output & Install (Steps A4–A8)

This file is loaded when search results are ready. Read it with `read_file` at the start of Step A4.

---

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
- Role-specific defaults from the role file already in context (Personalization Defaults section)
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

**1b — PROFILE ENRICHMENT FROM OUTLOOK (optional)**

After showing the profile, offer to enrich it with real work context from Outlook. This is the highest-value personalisation step — it turns a generic role profile into one that reflects the user's actual daily work.

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

If 1 or 2: append selected sentences to the PERSONAL INSTRUCTIONS block under a **"Work context (from Outlook)"** subsection. Show the updated block with `➕ Added:` markers. Update `joule-work-tailor/my-joule-work-tailor.md`.

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

Save full output (profile + recommendations) as `joule-work-tailor/my-joule-work-tailor.md` using `write_file`. For roles with DEMOS flag, append a **DEMO ENVIRONMENT QUICK-REFERENCE** section listing all DEMO_CONNECTOR_LIST entries (name, landscape, MCP endpoint) plus access instructions: "Before connecting: close ALL browser tabs. Log in fresh at experience-agent.cloud.sap with your own or demo user. Then add each connector."

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

4. Update `joule-work-tailor/my-joule-work-tailor.md` with the improved profile blocks (overwrite the profile section, keep the recommendations).

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
For option 3 (done): *"Your profile and recommendations are saved in joule-work-tailor/my-joule-work-tailor.md. Come back anytime to install. Open a new conversation to apply your new personalization profile."* End.

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

---

**PROFILE REFINEMENT (optional) — after install**

After the install summary, offer profile refinement:

*"Want to sharpen how Joule answers you? I can ask a few quick questions about your preferences — takes under a minute."*

1. Yes — ask me the refinement questions
2. No — I'm done

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
4. Update `joule-work-tailor/my-joule-work-tailor.md` with the refined profile.

If 2: continue to Step A7.

---

**Save state (silent — no output to user):**

After the install summary (and retry if applicable), save the full state to `joule-work-tailor/joule-work-tailor-state.json` using `write_file`. Build the JSON object with:

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
2. No — the results are already saved in `joule-work-tailor/my-joule-work-tailor.md`

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

