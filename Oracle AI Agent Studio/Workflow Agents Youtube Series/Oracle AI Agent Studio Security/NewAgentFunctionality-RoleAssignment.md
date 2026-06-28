# ➕ Adding New Agent Functionality – Role Assignment Guide

> **How to Extend Security When New AI Agents or Modules Are Added**

---

## 📖 Overview

Oracle continuously releases **new AI agents embedded into Oracle Fusion Cloud** products. Each new agent or module (e.g., Ledger Agent, Payables Agent) requires **additional security roles** to be assigned to users before they can access or use the agent.

This guide explains:
- The two-tier role model for agent-specific access
- How to extend existing custom roles to include new agent access
- A repeatable process for onboarding new agents as Oracle releases them

---

## 🧠 Understanding the Two-Tier Role Model

When Oracle releases a new agent, access is controlled at **two levels**:

```
┌───────────────────────────────────────────────────────┐
│       TIER 1: AI Agent Studio Access                    │
│  (Can the user open/configure the agent in Studio?)     │
│  Roles: FAI Administrator Duties (for Builders)         │
│         FAI Runtime Duty + Chat Priv (for Explorers)    │
└───────────────────────────────────────────────────────┘
                          │
                          ▼
┌───────────────────────────────────────────────────────┐
│       TIER 2: Agent-Specific Functional Access          │
│  (Can the user access the specific agent's data/page?)  │
│  Roles: Agent-specific duty roles                       │
└───────────────────────────────────────────────────────┘
```

> **Both tiers must be satisfied** for a user to fully access a specific agent.

---

## 📊 Agent-Specific Role Requirements

Instead of hardcoding specific role names which are subject to change with each release, always refer to the **Oracle Cloud Readiness** documentation or the **Security Reference Guides** for the exact duty roles required for new agents.

### Example: Financial Agents (Ledger Agent, Payables Agent)

When enabling Financial agents, follow this general framework:

#### Tier 1 Access (AI Agent Studio / Platform)
- **Builders:** `ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY`
- **Explorers:** `ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY` + `HRC_ACCESS_AI_AGENT_CHAT_PRIV`

#### Tier 2 Access (Agent-Specific Functional Roles)
- **Ledger Agent:** Refer to the latest Oracle General Ledger documentation for the specific Ledger Inquiry or Ledger Agent duty roles required.
- **Payables Agent:** Refer to the latest Oracle Payables documentation for the specific Payables Agent or AP Intelligent Agent duty roles required.

#### Additional Data Security Required
- AI Agents inherit existing Oracle Fusion data security.
- **Data Access Sets:** Users must be assigned the appropriate Data Access Set for the Ledgers they want to query.
- **Business Unit Assignment:** Users will only see Payables/Invoice data for Business Units they already have access to.

---

## 🔄 Repeatable Process: Onboarding Any New Oracle AI Agent

Whenever Oracle releases a new AI agent, follow this standard process:

```
┌────────────────────────────────────────────────────┐
│  STEP 1: Identify the new agent's product pillar         │
│          (HCM, FIN, SCM, PRC, CX, PSC, etc.)            │
└─────────────────────────┬──────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────┐
│  STEP 2: Check Oracle Cloud Readiness / Release Notes    │
│          for the EXACT new agent-specific duty roles     │
└─────────────────────────┬──────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────┐
│  STEP 3: Add new agent-specific roles to the            │
│          existing Custom Job Role in Security Console    │
└─────────────────────────┬──────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────┐
│  STEP 4: Run Security Sync Scheduled Processes          │
│          (Import Resource + Import User/Role Data)       │
└─────────────────────────┬──────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────┐
│  STEP 5: In AI Agent Studio, go to Agent Teams          │
│          Security tab → Add new role or toggle          │
│          'For All Employees'                            │
└─────────────────────────┬──────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────┐
│  STEP 6: Test with a pilot user before broad rollout   │
└────────────────────────────────────────────────────┘
```

---

## 📜 Master Role Assignment Template

Use this template to track role assignments when a new Oracle AI agent is released:

```
Agent Name: ___________________________
Product Pillar: ________________________
Oracle Release: ________________________
Date Added: ____________________________

TIER 1 ROLES (AI Agent Studio Access):
[ ] FAI Admin Duty Role (for Builders): _________________________
[ ] FAI Runtime Duty (for Explorers):   ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY
[ ] Chat Privilege (for Explorers):      HRC_ACCESS_AI_AGENT_CHAT_PRIV

TIER 2 ROLES (Agent-Specific Functional Access):
[ ] Role Name: _________________________  Code: __________________
[ ] Role Name: _________________________  Code: __________________

DATA SECURITY:
[ ] Data Access Set configured: Yes / No
[ ] Business Unit restriction: Yes / No / N/A

AGENT TEAM SECURITY:
[ ] Added role to Agent Team: Yes / No
[ ] 'For All Employees' toggle: On / Off

SECURITY SYNC JOBS RUN:
[ ] Import Resource Application Security Data: Done
[ ] Import User and Role Application Security Data: Done

USERS ASSIGNED:
[ ] User list: _________________________
[ ] Tested with pilot user: Yes / No
```

---

## 🛠️ Where to Find New Roles After an Oracle Upgrade

When Oracle releases a new version, check these sources for new agent-specific roles:

| Source | How to Access |
|--------|---------------|
| **Oracle Cloud Readiness** | [cloud.oracle.com/readiness](https://www.oracle.com/cloud/readiness/) → Filter by Fusion Apps → AI / Agent Studio |
| **Oracle Help Center** | [docs.oracle.com/en/cloud/saas/fusion-ai](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/) |
| **Security Console → Role Search** | Search for new `ORA_DR_FAI_*` or `ORA_FAI_*` role codes after upgrade |
| **Oracle Security Reference Guides** | Available per product (FIN, HCM, SCM) on Oracle Help Center |
| **Oracle Support Notes** | Search [support.oracle.com](https://support.oracle.com) for agent security notes |

---

## 📚 Official Reference Links

| Topic | Link |
|-------|------|
| Oracle Cloud Readiness (New Features) | [cloud.oracle.com/readiness](https://www.oracle.com/cloud/readiness/) |
| AI Agent Studio Documentation | [Oracle Docs – 26B](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/) |
| Access Requirements Overview | [Access AI Agent Studio](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-ai-agent-studio.html) |

---

*Last updated: June 2026 | Oracle Fusion Cloud Release 26B*