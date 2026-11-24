# Phase: Intake (Steps A1–A2b)

This file is loaded when the user enters Flow A intake. Read it with `read_file` at the start of Step A1.

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
- In Step A3, run the base skill queries for SECONDARY_ROLE (from the role file already in context — Skill Queries → Base section only). Merge results into the main recommendation list. Deduplicate by UUID/name.
- In tiering: skills that appear in both primary and secondary base queries → promote to Essential. Skills only in secondary → Recommended unless already tiered higher.
- In Step A5 profile generation: the Role Description should lead with the primary role and add one short line for the secondary (e.g. "Also covers [secondary role name]"). Keep within the 480-char limit — trim secondary context if needed. The Personal Instructions block should include defaults from both roles, deduplicated.
- SECONDARY_ROLE connectors: include ESSENTIAL CONNECTORS from secondary role's MCP Queries (base only). Do NOT include demo environment connectors for secondary role unless the same landscape is already in DEMO_ENVS.
- If the secondary role triggers the ADAPT Connector Rule (Roles 1, 7, 9, 10 in INTERNAL; Roles 1, 9, 12 in PARTNER): apply it.

### Step A2 — Role-Specific Structured Questions

The role file for ROLE is already in context from the Intake phase — use it directly for the question sequence.
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

### Step A2b — Proceed to Search

After collecting all answers, announce immediately — no confirmation question:

*"Got it — searching the catalog for **[ROLE]** covering [AREAS in plain language] with [activities in plain language]."*

Then load `references/phase-search.md` and begin Step A3.

**Correction escape hatch:** If the user then says "wait", "actually", "I made a mistake", or similar — stop, ask which question they want to fix (Q1 / Q2 / Q3), re-collect that answer, then continue to A3 again.

