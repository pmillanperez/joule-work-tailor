# SIX Demo Environments — Reference Catalog

## Overview

SIX (SAP Interactive Experience) demo landscapes are full SAP system groups — a leading S/4HANA system paired with connected Line-of-Business cloud solutions. Each landscape exposes its functionality via MCP connectors that Joule can use directly in customer-facing conversations and demos.

All SIX environments require authentication via the **SIX Experience Agent**. Connect the Experience Agent connector first, before connecting any landscape connector.

- **SIX Hub:** https://six-mcp-hub.cfapps.eu10-004.hana.ondemand.com/#landscapes
- **Experience Agent login:** https://experience-agent.cloud.sap
- **Experience Agent MCP:** https://ngs-core-prod.c-0fd87d1.kyma.ondemand.com/api/v1/mcp-server/exa

---

## Prerequisites

1. Install the **SIX Experience Agent** connector in Joule Work (Extensions → Connectors → New).
2. Log in at experience-agent.cloud.sap with your own user or a demo user before starting a conversation.
3. **Close existing browser sessions** before connecting — active login cookies may cause SSO to authenticate with the wrong user.
4. Demo users have pre-configured roles. Personal users must assign the required business scenario roles manually.

---

## Landscape Catalog

### S/4HANA Public Cloud Landscapes

#### C39 — S/4HC 3SL EMEA Standard
**Type:** Public Cloud · **Stability:** Long-term
**Best for:** EMEA standard Public Cloud ERP; BDC/Datasphere; CX (Sales, Service, Engagement); Asset Management; GRC; Digital Manufacturing; Finance/Treasury

| # | Service | MCP Endpoint | Areas |
|---|---|---|---|
| 1 | S/4HANA Public Cloud | https://se-demo-six-c39-oid-mcp-mcp-s4hana-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP / Finance |
| 2 | Field Service & Asset Management | https://se-demo-six-c39-oid-mcp-mcp-fsm-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Supply Chain |
| 3 | Asset Performance Management | https://se-demo-six-c39-oid-mcp-mcp-apm-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Supply Chain |
| 4 | SAP Sales & Service Cloud | https://se-demo-six-c39-oid-mcp-mcp-ss-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | CX |
| 5 | SAP Digital Manufacturing | https://se-demo-six-c39-oid-mcp-mcp-dmc-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Supply Chain |
| 6 | SAP Datasphere in Business Data Cloud | https://se-demo-six-c39-oid-mcp-mcp-dsp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | BAIP Data and AI |
| 7 | SAP Engagement Cloud | https://se-demo-six-c39-oid-mcp-mcp-ecee-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | CX |
| 8 | SAP Integrated Product Development | https://se-demo-six-c39-oid-mcp-mcp-ipd-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Supply Chain |
| 9 | SAP GRC — Risk & Assurance Management | https://se-demo-six-c39-oid-mcp-mcp-grc-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cross-portfolio |

*Coming soon: SAP Commerce Cloud, SAP SuccessFactors, SAP Ariba*

---

#### NQO — S/4HC 3SL US Standard
**Type:** Public Cloud · **Stability:** Long-term
**Best for:** North America standard Public Cloud ERP scenarios

| # | Service | MCP Endpoint | Areas |
|---|---|---|---|
| 1 | S/4HANA Public Cloud | https://se-demo-six-nqo-oid-mcp-mcp-s4hana-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP |

---

#### D2P — S/4HC 3SL Custom II
**Type:** Public Cloud · **Stability:** Long-term
**Best for:** SuccessFactors (HCM) and Ariba (Finance & Spend) combined with ERP

| # | Service | MCP Endpoint | Areas |
|---|---|---|---|
| 1 | S/4HANA Public Cloud | https://se-demo-six-d2p-oid-mcp-mcp-s4hana-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP |
| 2 | SAP SuccessFactors | https://se-demo-six-d2p-oid-mcp-mcp-sfsf-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | HCM |
| 3 | SAP Ariba Sourcing & Contracts | https://se-demo-six-d2p-oid-mcp-mcp-ariba-mcp-sourcing-contract570e32f0.cfapps.eu10-005.hana.ondemand.com/mcp | Finance & Spend |
| 4 | Ariba Data Extractor | https://se-demo-six-d2p-oid-mcp-mcp-ariba-data-extractor-mcp.cfapps.eu10-005.hana.ondemand.com/mcp | Finance & Spend |
| 5 | SAP Ariba SLP | https://se-demo-six-d2p-oid-mcp-mcp-ariba-slp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Finance & Spend |

---

#### JI4 — S/4HC 3SL EMEA Custom
**Type:** Public Cloud · **Stability:** Long-term
**Best for:** CX + Commerce + Subscription Billing + Revenue Management — XaaS/subscription business model demos

| # | Service | MCP Endpoint | Areas |
|---|---|---|---|
| 1 | S/4HANA Public Cloud | https://se-demo-six-ji4-oid-mcp-mcp-s4hana-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP |
| 2 | SAP Sales & Service Cloud | https://se-demo-six-ji4-oid-mcp-mcp-ss-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | CX |
| 3 | SAP Commerce Cloud | https://se-demo-six-ji4-oid-mcp-mcp-commerce-pcm-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | CX |
| 4 | Entitlement Management | https://se-demo-six-ji4-oid-mcp-mcp-ems-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | CX |
| 5 | SAP Subscription Billing | https://se-demo-six-ji4-oid-mcp-mcp-sb-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP / CX |
| 6 | SAP Engagement Cloud | https://se-demo-six-ji4-oid-mcp-mcp-ecee-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | CX |

---

#### K6D — Service Industry
**Note:** Endpoints via SharePoint only — no direct MCP URL. Access via Experience Agent.

#### P1A — S4HC Public Sector
| # | Service | MCP Endpoint | Areas |
|---|---|---|---|
| 1 | S/4HANA Public Cloud | https://se-demo-six-p1a-oid-mcp-e3d1qpbz-mcp-s4hana-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP / Public Sector |

#### CWF — S4HC Financial Services
| # | Service | MCP Endpoint | Areas |
|---|---|---|---|
| 1 | S/4HANA Public Cloud | https://se-demo-six-cwf-oid-mcp-thypo3a6-mcp-s4hana-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP / Financial Services |

#### CFS — S4HC 3SL EMEA Regional Long-Term
| # | Service | MCP Endpoint | Areas |
|---|---|---|---|
| 1 | S/4HANA Public Cloud | https://se-demo-six-cfs-oid-mcp-qftqb9mh-mcp-s4hana-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP |

#### I4M — Public Sector (Public Cloud)
| # | Service | MCP Endpoint | Areas |
|---|---|---|---|
| 1 | S/4HANA Public Cloud | https://se-demo-six-i4m-oid-mcp-mcp-s4hana-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP / Public Sector |

> ⚠️ Note: if the above endpoint returns a connection error, verify the landscape ID against the SIX Hub at https://six-mcp-hub.cfapps.eu10-004.hana.ondemand.com/#landscapes — the URL may differ from the landscape code.

---

### S/4HANA Private Cloud Landscapes

#### GSA — Weekly Refreshed Demo
**Type:** Private Cloud · **Stability:** Weekly refresh — data resets; do not use for extended PoC

| # | Service | MCP Endpoint | Areas |
|---|---|---|---|
| 1 | S/4HANA Private Cloud (HE4) | https://se-demo-six-gsa-oid-mcp-mcp-s4hana-op-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP Private |
| 2 | S/4HANA Utilities (S4U) | https://se-demo-six-gsa-oid-mcp-mcp-s4hana-s4u-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP / Utilities |
| 3 | S/4HANA BRIM (S4B) | https://se-demo-six-gsa-oid-mcp-mcp-s4hana-s4b-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP / Revenue Mgmt |
| 4 | S/4HANA Public Sector (S4P) | https://se-demo-six-gsa-oid-mcp-mcp-s4hana-s4p-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP / Public Sector |
| 5 | SAP Commerce Cloud | https://se-demo-six-gsa-oid-mcp-mcp-commerce-pcm-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | CX |

---

#### GLA-26Q2 — Long-Term Demo Landscape (recommended for extended demos)
**Type:** Private Cloud · **Stability:** Long-term — suitable for extended PoC

| # | Service | MCP Endpoint | Areas |
|---|---|---|---|
| 1 | S/4HANA Private Cloud (HE4) | https://se-demo-six-gla26-oid-mcp-mcp-s4hana-op-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP Private / Finance |
| 2 | S/4HANA Utilities (S4U) | https://se-demo-six-gla26-oid-mcp-mcp-s4hana-s4u-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP / Utilities |
| 3 | S/4HANA BRIM (S4B) | https://se-demo-six-gla26-oid-mcp-mcp-s4hana-s4b-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP / Revenue Mgmt |
| 4 | S/4HANA Public Sector (S4P) | https://se-demo-six-gla26-oid-mcp-mcp-s4hana-s4p-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Cloud ERP / Public Sector |
| 5 | SAP IBP | https://se-demo-six-gla26-oid-mcp-mcp-ox5-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Supply Chain |
| 6 | Sales & Service Cloud (Cross-industry) | https://se-demo-six-gla26-oid-mcp-mcp-cross-ss-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | CX |
| 7 | Sales & Service Cloud (Utilities) | https://se-demo-six-gla26-oid-mcp-mcp-utilities-ss-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | CX |
| 8 | SAP Digital Manufacturing | https://se-demo-six-gla26-oid-mcp-mcp-dmc-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Supply Chain |
| 9 | SAP Commerce Cloud | https://se-demo-six-gla26-oid-mcp-mcp-commerce-pcm-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | CX |
| 10 | SAP Integrated Product Development | https://se-demo-six-gla26-oid-mcp-mcp-ipd-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Supply Chain |
| 11 | SAP Datasphere in Business Data Cloud | https://se-demo-six-gla26-oid-mcp-mcp-dsp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | BAIP Data and AI |
| 12 | Asset Performance Management | https://se-demo-six-gla26-oid-mcp-mcp-apm-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Supply Chain |
| 13 | Direct Material Sourcing | https://se-demo-six-gla26-oid-mcp-mcp-s4-dms-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Finance & Spend |
| 14 | Field Service & Asset Management | https://se-demo-six-gla26-oid-mcp-mcp-fsm-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Supply Chain |
| 15 | Field Service & Asset Management (Utilities) | https://se-demo-six-gla26-oid-mcp-mcp-s4u-fsm-fsm-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp | Supply Chain |

*Coming soon: SAP SuccessFactors, SAP Ariba, SAP Concur, SAP Fieldglass*

---

### SAP IBP Cloud — Dedicated Supply Chain Planning Tenants

For deep IBP-only demos (no S/4HANA core needed).

#### IBP — NA Demo Tenants
| Tenant | MCP Endpoint |
|---|---|
| ZYB | https://se-demo-six-ibp-na-oid-mcp-tjh8ze73-mcp-zyb-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |
| Z2E | https://se-demo-six-ibp-na-oid-mcp-tjh8ze73-mcp-z2e-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |
| ZO4 | https://se-demo-six-ibp-na-oid-mcp-tjh8ze73-mcp-zo4-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |
| NFD | https://se-demo-six-ibp-na-oid-mcp-tjh8ze73-mcp-nfd-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |
| ZN3 | https://se-demo-six-ibp-na-oid-mcp-tjh8ze73-mcp-zn3-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |

#### IBP — EMEA Demo Tenants
| Tenant | MCP Endpoint |
|---|---|
| NFL | https://se-demo-six-ibp-emea-oid-mcp-a2nadwku-mcp-nfl-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |
| ZYN | https://se-demo-six-ibp-emea-oid-mcp-a2nadwku-mcp-zyn-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |
| Z1Q | https://se-demo-six-ibp-emea-oid-mcp-a2nadwku-mcp-z1q-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |

#### IBP — ANZ Demo Tenants
| Tenant | MCP Endpoint |
|---|---|
| OW3 | https://se-demo-six-ibp-anz-oid-mcp-mcp-ow3-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |
| OVY | https://se-demo-six-ibp-anz-oid-mcp-mcp-ovy-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |

#### IBP — POC Tenants
| Tenant | MCP Endpoint |
|---|---|
| ZWN | https://se-demo-six-ibp-poc-zwn-oid-mcp-mcp-poc-zwn-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |
| ZTA | https://se-demo-six-ibp-poc-zta-oid-mcp-mcp-poc-zta-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |
| ZJ5 | https://se-demo-six-ibp-poc-zj5-oid-mcp-mcp-poc-zj5-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |
| OW2 | https://se-demo-six-ibp-poc-ow2-oid-mcp-mcp-poc-ow2-ibp-mcp-server.cfapps.eu10-005.hana.ondemand.com/mcp |

---

### Life Sciences

#### Life Sciences — Long-Term Demo
**Best for:** Pharma, Life Sciences, Clinical Supply Management
- BTP-Life Science (SAP Intelligent Clinical Supply Management) — Live
- S4OP (S4L) — Live

*Full MCP endpoints: check https://six-mcp-hub.cfapps.eu10-004.hana.ondemand.com/#landscapes*

---

## Role-Based Landscape Selection Guide

Use this table in Q3 to filter which landscapes to show based on TABLE A AREAS selection.

| User AREAS (TABLE A) | Recommended landscapes |
|---|---|
| 1 Cloud ERP (standard) | C39, GLA-26Q2, GSA, CFS (EMEA LT), NQO (US) |
| 1 Cloud ERP (Utilities) | GLA-26Q2 (S4U), GSA (S4U) |
| 1 Cloud ERP (BRIM / Revenue Mgmt) | GLA-26Q2 (S4B), GSA (S4B), JI4 (Subscription Billing) |
| 1 Cloud ERP (Public Sector) | P1A, I4M, GLA-26Q2 (S4P), GSA (S4P) |
| 1 Cloud ERP (Financial Services) | CWF |
| 2 BAIP — Data and AI | C39 (dsp), GLA-26Q2 (dsp) |
| 2 BAIP — Technology Foundation | No dedicated SIX landscape — BTP/AI Foundation available in all landscapes |
| 3 CX (Sales, Service, Engagement) | C39 (ss, ecee), JI4 (ss, ecee, commerce), GLA-26Q2 (cross-ss) |
| 3 CX (Commerce, Subscriptions) | JI4 (commerce, sb, ems), GLA-26Q2 (commerce), GSA (commerce) |
| 4 Supply Chain (IBP) | GLA-26Q2 (ibp), IBP dedicated tenants (NA/EMEA/ANZ/POC) |
| 4 Supply Chain (Mfg, Field Svc, APM) | C39 (dmc, fsm, apm), GLA-26Q2 (dmc, fsm, apm) |
| 5 Finance & Spend | C39 (S/4HANA Finance), GLA-26Q2 (S/4HANA Finance + s4-dms), D2P (ariba, ariba-slp) |
| 6 HCM / SuccessFactors | D2P (sfsf) |
| 7 BTM | No dedicated SIX landscape — LeanIX, Signavio, WalkMe have dedicated cloud tenants; contact BTM demo team |
| 8 All / Cross-portfolio | C39, GLA-26Q2, GSA, D2P, JI4 |

---

## Paired Skills per Demo Scenario

| Scenario | Paired Skills |
|---|---|
| BDC Working Capital Intelligence (GLA-26Q2 / C39 dsp) | wci-demo-guide, sap-bdc-advisor |
| BDC People Intelligence / HR Analytics (C39 dsp) | sap-bdc-advisor |
| Any SIX demo — customer preparation | customer-prep-presales |
| Post-demo content — branded customer deck | sap-corporate-pptx |
| Post-demo content — tailored proposal or technical docs | sap-bdc-advisor, sap-s4hana-consultant |
| Finding reusable SA content for demo follow-up | adapt-use-case-search |
| IBP supply chain demos | sap-btp-consultant (for integration scenarios) |

---

## How to Personalize a Demo for a Customer

1. **Before the demo:** Use `customer-prep-presales` to research the customer and align the demo flow to their context, industry, and pain points.
2. **Scenario filtering:** Specify the customer's industry and key challenge in the conversation. The MCP server exposes pre-loaded demo data — ask Joule to query for the relevant scenario.
3. **Data and AI angle:** For BDC or Datasphere scenarios, use `wci-demo-guide` or `sap-bdc-advisor` for step-by-step talking points.
4. **Post-demo follow-up:** Use `sap-corporate-pptx` to generate a branded deck mirroring the demo narrative, or `adapt-use-case-search` to find reusable SAP content for a proposal.

## How to Build or Request a New Demo Environment

- **Standard SIX landscape:** https://six-mcp-hub.cfapps.eu10-004.hana.ondemand.com/#landscapes
- **Custom BDC/Datasphere space:** Use `sap-bdc-advisor` and `data-product-mentor` skills.
- **Custom Joule Studio agent or demo:** Use `teched26-joule-studio` and `sap-ai-core` skills.
- **BTM demo (LeanIX, Signavio, WalkMe):** Contact your BTM SA or demo team for dedicated cloud tenant access.
- **Customer-specific PoC:** Contact your regional demo team or use `oneai-space-manager` skill for shared PoC spaces.