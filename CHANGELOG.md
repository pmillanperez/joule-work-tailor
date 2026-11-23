# Joule Work Tailor — Changelog

## v2.0.1 — November 2026

### Fixed
- **Customer Track state persistence** — CT4 now saves `joule-work-tailor-state.json` with the same schema as Flow A/B. Customer users get persistent dismiss, NEW badges, and skipped-skill tracking across sessions.
- **Customer Track returning-user path** — added CT0 (returning customer fast path) and CT-B (refresh flow). Returning customers now see "Refresh my toolkit / Full setup" instead of repeating first-time setup every session.
- **Outlook enrichment empty-result handler** — if `list_emails` and `list_calendar_events` return 0 results, the skill now shows a graceful message and sets `PROFILE_ENRICHED = false` instead of attempting to generate enrichment sentences from empty data.
- **Fallback C role query** — broader search now uses the user's actual role name from A1 (e.g. `search_skills(query: "<ROLE name>")`) instead of a hardcoded "Solution Advisor" example.
- **Match column for non-Hub items** — GitHub skills and AI-generatable templates display "—" in the Match column instead of a blank or hallucinated score.

### Changed
- **SKILL.md** expanded from ~1,124 to ~1,198 lines

---

## v2.0.0 — November 2026

### New
- **State persistence** — saves user profile to `joule-work-tailor-state.json` (role, tier, areas, activities, installed skills/connectors, dismissed items, timestamps). Returning users skip intake questions and see a delta report of what's new since last run.
- **NEW / Updated badges** — skills added or updated in the Hub catalog since the user's last setup are flagged with `🆕 NEW` or `🔄 Updated` in all recommendation tables.
- **Dismiss permanently** — `dismiss N` hides an unwanted recommendation; `undo dismiss` reverses the last dismissal; `show dismissed` lists all dismissed items with restore option.
- **Outlook enrichment** — reads recent emails (`list_emails`) and calendar events (`list_calendar_events`) to detect SAP product mentions, meeting patterns, and customer context. Enrichment signals feed into area overlap and activity scoring.
- **Match scoring (%)** — 5-component quantitative formula: tier base (40%), area overlap (25%), activity signals (15%), community popularity (10%), recency (10%). Displayed as a `Match: XX%` badge on every recommendation row.
- **Fallback chain** — 3-tier relaxation when strict filters yield ≤ 2 results: Tier 1 drops activity constraint, Tier 2 drops area constraint, Tier 3 shows a curated cross-role list. Each tier is labelled to explain why it appeared.

### Changed
- **allowed-tools** — added `list_emails`, `list_calendar_events`, `read_file` for Outlook enrichment and state persistence
- **Tags** — added `state-management`, `scoring`, `outlook-enrichment`
- **SKILL.md** expanded from ~860 to ~1,124 lines

---

## v1.9.3 — November 2026

### Changed
- **Renamed** — skill slug changed from `jwd-tailor` to `joule-work-tailor` and display name from "jwd-tailor" to "Joule Work Tailor" for public catalog discoverability. Output files renamed accordingly (`my-joule-work-tailor.md`, `my-joule-work-tailor-update.md`). All internal references, documentation, and GitHub Contribution Guide updated.

---

## v1.9.2 — November 2026

### Fixed
- **Partner Mode — SIX demo gate for Role 5** — Data & AI Specialist (Role 5) in PARTNER mode was bypassing the SIX partner access gate and showing C39/GLA-26Q2 landscape options directly. Added Role 5 to the gated roles list (now Roles 1, 5, 9, 12).

---

## v1.9.1 — November 2026

### New
- **Profile Refinement (Step A5 — 1b)** — after the generated profile is shown, the skill now offers an optional 5-question refinement sequence (R1–R5) covering response language, format, technical depth, length, and tone. Selections are translated into concrete instructions and appended to the Personal Instructions block as a "Format and style preferences" subsection.

---

## v1.9.0 — November 2026

### New
- **Step A8 / CT5** — Space creation offer at the end of setup: creates a dedicated 'My JWD Toolkit' space with profile, skills, and connector cards
- **Context warning** — Step 0 now warns users to run the skill from the main Joule chat, not from inside an existing project Space
- **EXTERNAL auto-install** — EXTERNAL flow now attempts GitHub → `create_skill` auto-install before falling back to manual download instructions
- **Source legend** — emoji-based legend (✅ SAP Official / 🌐 Community / 🐙 GitHub / 🤖 Create with AI) now appears above recommendation tables in both Flow A (Step A5) and Flow B (Step B4)

### Changed
- **Shared Knowledge Bases** — OneAI/agents section renamed; skipped entirely in PARTNER, GENERAL, and CUSTOMER modes; description added explaining what the platform is and who it is for
- **CT4** — references new two-step EXTERNAL install flow
- **Partner Mode Adjustments** — install description updated to reflect auto-install attempt
- **allowed-tools** — added `create_space` and `add_card_to_space`
- **Tags** — cleaned up to 10 canonical tags

---

## v1.8.1 — November 2026

### Fixed (reference files — 65 findings across 5 files)
- **demo-environments.md** — I4W label correction, canonical area tags, IBP table expansion, K6D/Life Sciences warnings, missing section headers, HE4 stripping, ariba-data-extractor added, prerequisites Step 4 (10 fixes)
- **sap-portfolio.md** — SAP WalkMe naming, HXM retirement note, GROW deduplication, HANA Cloud rule, Joule status annotation, AI Foundation cross-ref, Partner/Customer Roles section, SAP APM row (8 fixes)
- **roles.md, agents.md, SKILL.md** — content accuracy and consistency fixes across all 5 files (47 fixes)

---

## v1.7.0 — November 2026

### New
- **3 audience tracks** — SAP internal (hub.joule.only.sap), SAP partner consultants, SAP customer business users
- **17 roles** — expanded from 11; added Role 12 (Partner Delivery / Implementation Consultant) and Roles 13–17 (Customer: IT Admin, Finance, HR, Operations, Data Analyst / General)
- **Step 0 audience detection** — auto-branches into INTERNAL / PARTNER / CUSTOMER before showing options
- **Partner Mode** — SIX demo environment access gating; ADAPT connector auto-surfaced for advisory roles (1, 9, 12)
- **Customer Track (CT1–CT5)** — clean presales-free onboarding flow using public catalog only; excludes demo environment connectors, SIX endpoints, and presales-centric skills
- **Resilient install engine** — continues on failure across all items, shows a structured INSTALL SUMMARY, offers one-click retry; OAuth / SSO session conflict guidance included

### Fixed
- Added `web_search` to `allowed-tools` frontmatter — resolves agentskills.io spec compliance gap (tool was used in EXTERNAL mode but not declared)

### Changed
- ADAPT Connector Rule formalised: prepend to ESSENTIAL CONNECTORS for roles 1, 7, 9, 10 (INTERNAL) and roles 1, 9, 12 (PARTNER)
- Source labels (SAP Official / Community) now required on every skill and connector row in recommendation tables and update reports

---

## v1.6.6 — October 2026

- Added Source column (SAP Official / Community) to recommendation and update report tables
- Added Step A2b personalisation profile generator (Language, Format, Detail, Custom Rules)
- Added Step A2c profile summary confirmation before running catalog searches
- Improved demo environment connector handling: Experience Agent always installed first
- OAuth error guidance added to install flow

---

## v1.6.0 — September 2026

- Initial public release
- Roles 1–11 across SAP employee tracks
- First-time setup (Flow A) and stay current (Flow B) modes
- Live catalog search via Hub MCP in INTERNAL mode
- Personalization profile generator (Role Description + Personal Instructions)
