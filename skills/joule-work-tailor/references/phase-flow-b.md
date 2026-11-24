# Phase: Stay Current (Flow B)

This file is loaded when the user selects 'Stay current' in Step 1. Read it with `read_file` at the start of Flow B.

**IMPORTANT:** The role file (`references/role-{NN}.md`) is loaded alongside this file by SKILL.md routing — it is already in context. Use it directly for Skill Queries and Personalization Defaults. Do NOT read `references/roles.md`.

---

## Flow B — Stay Current

### Step B1 — Audit

Fire in a **single message** (INTERNAL only):
- `mcp_<hash>_list_my_skills()` + `mcp_<hash>_check_updates(skills: [...])`
- If `joule-work-tailor/joule-work-tailor-state.json` was not already loaded in Step 0, read it now.

Store OUTDATED and INSTALLED_UUIDS. Merge `installed_uuids` from state (if available) with the live `list_my_skills` result — the union gives the most complete installed set.

### Step B2 — Quick Check

**If STATE = loaded (returning user — most common path for Flow B):**

Skip the role question and the "anything new" question entirely — use stored role, areas, and flags. Ask only:

**Refresh how Joule answers you?**

1. Yes — regenerate and show what changed
2. No — skip to new skills

*Type one number.*

If 1:
- Read `joule-work-tailor/my-joule-work-tailor.md` if it exists → set PREV_PROFILE.
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
- Run file check: does `joule-work-tailor/my-joule-work-tailor.md` exist? If yes, read and set PREV_PROFILE.
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
Save as `joule-work-tailor/my-joule-work-tailor-update.md`.

**How would you like to proceed?**

1. Update everything automatically
2. Choose what to update (type numbers)
3. I’m done — I’ll install later

*Type one number.*

> Say **dismiss N** to permanently hide a skill. Dismissed skills never appear again.

Apply the same **resilient install flow** as Step A6 (continue on failure, show summary, offer retry).

**Save state (silent — no output to user):**

After the install summary, save full state to `joule-work-tailor/joule-work-tailor-state.json` — same schema and merge logic as the Flow A state save (after Step A6). Set `last_run_mode` to `"update"`. Update `last_seen_catalog` with `current_catalog_uuids`. Merge newly installed UUIDs into `installed_uuids`. Update `skipped_skills` with skills shown but not installed.

---

