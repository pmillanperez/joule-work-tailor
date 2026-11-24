# Joule Work Tailor

**Set up Joule Work for your role — in under 5 minutes.**

Joule Work Tailor finds and installs the right skills, connectors, demo environments, personalization profile, and agents — just answer numbered questions.

---

## Who is it for?

| Audience | Track | What you get |
|---|---|---|
| **SAP employees** | Internal | Skills from hub.joule.only.sap, shared SAP knowledge bases, demo environment connectors, personal AI agent, personalization profile |
| **SAP partner consultants** | Partner | Skills from the public catalog, GitHub skills for partner delivery, demo connectors (if SIX access confirmed), personalization profile |
| **SAP customer business users** | Customer | Skills from the public catalog tailored to Finance, HR, Operations, IT, or Data Analyst roles — no presales content |

---

## What it does

### 1. Role-based skill recommendations
Answer a few numbered questions (role → areas → activities → demo environments) and the skill searches the catalog and returns a tiered recommendation:

- **Essential** — must-haves for your role
- **Recommended** — strong secondary fit
- **Optional** — situational

Every item shows its source: ✅ **SAP Official** · 🌐 **Community** · 🐙 **GitHub** · 🤖 **Create with AI**

### 2. Multi-role support
Add a secondary role with zero extra questions. The skill merges both roles' skill lists and deduplicates — skills that appear in both are promoted to Essential.

### 3. Personalization profile — generated automatically
Generates a **Role Description** and **Personal Instructions** block tailored to your role and activities. Copy and paste into **Settings → Personalization**. Already have a profile? Paste it and the skill improves it rather than replacing it — with a full diff showing what was kept, updated, added, and removed.

### 4. Profile refinement (optional)
After install, an optional 5-question sequence lets you sharpen your Personal Instructions: response language, format, technical depth, length, and tone.

### 5. Demo environment connectors *(Solution Advisors, Data & AI, Partners)*
Looks up the exact MCP endpoints for your selected SIX demo landscapes (C39, GLA-26Q2, and others) and adds them to your recommendations. Always installs the Experience Agent connector first.

### 6. Shared knowledge bases *(SAP employees only)*
Surfaces relevant shared SAP OneAI spaces for your role — pre-loaded AI agents you can chat with immediately, no installation required.

### 7. GitHub skills *(employees and partners)*
Points you to curated skill repositories on github.tools.sap (internal) and github.com (public) that aren't in the Hub catalog.

### 8. AI-generated custom skills *(SAP employees)*
Generates personalised custom skills for you after the main install: SAP Product Expert, Presales Strategy Partner, Application Design Coach, or Demo Prep Assistant.

### 9. Personal OneAI agent *(SAP employees)*
Optionally creates a personal AI assistant space on OneAI, pre-configured for your role.

### 10. 'My JWD Toolkit' Space
At the end of setup, offers to create a persistent Joule Space with your profile, recommended skills, and connectors.

---

## Two modes

| Mode | When to use |
|---|---|
| **Set up my toolkit** | First time, or after a role change |
| **Stay current** | Check for new skills, updates, and refresh your profile |

---

## Supported roles (17 total)

**SAP employees and partners:** Solution Advisor · Enterprise Architect · Value Advisor · Delivery Consultant · Data & AI Specialist · Developer · Customer Success Manager · Product Manager · Partner Consultant · Business Development · Partner Delivery Consultant

**SAP customer users:** Finance/Accounting · HR/People · Operations/Supply Chain · IT/Admin · Data Analyst · General Knowledge Worker

---

## Installation

### From skills.cloud.sap
Search for `joule-work-tailor` and install directly.

### Via CLI
```bash
npx skills add pmillanperez/joule-work-tailor --skill joule-work-tailor
```

### Manual
Download the ZIP and go to **Extensions → Skills → New → Upload**.

### Requirements
- Joule Work Desktop
- For full setup (SAP employees): connected to hub.joule.only.sap
- For shared knowledge bases: connected to oneai-api.cfapps.eu10-004.hana.ondemand.com MCP
- For demo environments: SIX partner access (partners are asked to confirm before seeing landscape options)

---

## How to trigger it

**English:** `Joule Work Tailor` · `joule-work-tailor` · `equip my JWD` · `tailor JWD to my role` · `set up Joule Work for my role` · `tailor my Joule Work` · `personalize joule work` · `what skills should I install` · `set up my demo environments` · `set up Joule as a partner` · `I work at an SAP customer`

**German:** `JWD einrichten` · `Joule für meine Rolle anpassen` · `Joule Work personalisieren` · `welche Skills brauche ich`

**Spanish:** `personaliza mi joule work` · `qué necesito para mi rol`

---

## What's new in v3.0.1

- **Phase-based architecture** — skill files split into phases loaded on demand; faster responses, less LLM context per turn
- **Individual role files** — `role-01.md` through `role-17.md` replace the monolithic `roles.md`; only the selected role is loaded
- **Profile refinement moved** — R1–R5 questions now appear after install, not before recommendations
- **Streamlined intake** — Step A2b confirmation turn removed; inline announcement replaces the extra round-trip
- **State persistence, NEW/Updated badges, Outlook enrichment, Match scoring** — all features from v2.x carried forward

---

| | |
|---|---|
| **Author** | Pedro Millan · pedro.millan@sap.com |
| **Version** | 3.0.1 |
| **License** | Apache 2.0 |
| **Catalog** | [skills.cloud.sap](https://skills.cloud.sap) |

## Contributing

This skill is maintained in this repository. To suggest improvements, open an issue or submit a pull request.

## License

[Apache License 2.0](LICENSE)
