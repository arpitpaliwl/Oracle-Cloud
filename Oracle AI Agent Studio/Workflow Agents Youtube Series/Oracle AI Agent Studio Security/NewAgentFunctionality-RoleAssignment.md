# ➕ Adding New Agent Functionality – Role Assignment Guide

> **How to Extend Security When New AI Agents or Modules Are Added (e.g., Ledger Agent, Payables Agent)**

---

## 📖 Overview

Oracle continuously releases **new AI agents embedded into Oracle Fusion Cloud** products. Each new agent or module may require **additional security roles** to be assigned to users before they can access or use the agent.

This guide explains:
- The two-tier role model for agent-specific access
- Roles required for specific new agents (Ledger Agent, Payables Agent, etc.)
- How to extend existing custom roles to include new agent access
- A repeatable process for onboarding new agents as Oracle releases them

---

## 🧠 Understanding the Two-Tier Role Model

When Oracle releases a new agent (e.g., Ledger Agent), access is controlled at **two levels**:

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
│  Roles: Agent-specific duty roles (e.g., Ledger         │
│         Inquiry Assistant, Payables Agent Runtime)      │
└───────────────────────────────────────────────────────┘
```

> **Both tiers must be satisfied** for a user to fully access a specific agent.

---

## 📊 Agent-Specific Role Requirements

### 📊 Ledger Agent (General Ledger)

The **Ledger Agent** is an AI-powered assistant embedded in Oracle Fusion General Ledger that allows users to query ledger balances, journal entries, and account details using natural language.

#### Who Needs Access?
- General Accountants
- Financial Controllers
- Finance Managers
- Anyone using General Ledger who wants AI-powered ledger inquiries

#### Roles Required

| Tier | Role / Duty Name | Role Code | Tab |
|------|-----------------|----------|-----|
| Tier 2 | Ledger Inquiry Assistant | _(Predefined Oracle duty role)_ | Roles and Privileges |
| Tier 1 | Fai Genai Agent FIN Administrator Duty _(for Builders)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY` | Roles & Permission Groups |
| Tier 1 | Fai Genai Agent Runtime Duty _(for Explorers)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY` | Roles & Permission Groups |

#### Additional Data Security Required
- **Data Access Sets:** Users must be assigned the appropriate **Data Access Set** for the Ledger they want to query
- **Ledger or Business Unit Assignment:** The user's existing GL security (ledger set, balancing segment values) governs what data the Ledger Agent returns

#### Configuration Steps
```
1. Go to Security Console → Open the user's custom job role
2. Role Hierarchy → Roles and Privileges tab:
   → Add: Ledger Inquiry Assistant duty role
3. Role Hierarchy → Roles and Permission Groups tab:
   → Add: ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY (for Builders)
   OR
   → Add: ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY (for Explorers)
4. Verify the user has an appropriate Data Access Set for their ledger
5. Save the role
6. Assign to users
```

> 💡 **Note:** The Ledger Agent is embedded directly in the General Ledger workspace. Once the security is configured, the agent interface is available within the GL module — no separate standalone page is needed.

---

### 💰 Payables Agent (Accounts Payable)

The **Payables Agent** is an AI assistant embedded in Oracle Fusion Payables that helps users process invoices, validate suppliers, query payment status, and perform AP-related inquiries using conversational AI.

#### Who Needs Access?
- Accounts Payable Clerks
- AP Supervisors
- Invoice Processing Teams
- Finance Shared Services Teams

#### Roles Required

| Tier | Role / Duty Name | Role Code | Tab |
|------|-----------------|----------|-----|
| Tier 2 | Manage Financials Intelligent Agent | `ORA_FUN_MANAGE_FIN_AI_AGENT` | Roles and Privileges |
| Tier 2 | Manage Financials Intelligent Agent (HCM) | `ORA_FUN_MANAGE_FIN_AI_AGENT_HCM` | Roles and Privileges |
| Tier 1 | Fai Genai Agent FIN Administrator Duty _(Builders)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY` | Roles & Permission Groups |
| Tier 1 | Fai Genai Agent Runtime Duty _(Explorers)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY` | Roles & Permission Groups |

#### Additional Considerations
- **Business Unit Security:** The Payables Agent respects existing Business Unit restrictions. Users will only see AP data for Business Units they already have access to
- **Supplier Site Access:** Agent-driven supplier validation follows existing Payables security
- **Invoice Actions:** The agent can only perform actions (e.g., create invoice) if the user's role already permits those actions in standard Payables

#### Configuration Steps
```
1. Go to Security Console → Open the user's custom job role
2. Role Hierarchy → Roles and Privileges tab:
   → Add: ORA_FUN_MANAGE_FIN_AI_AGENT
   → Add: ORA_FUN_MANAGE_FIN_AI_AGENT_HCM
3. Role Hierarchy → Roles and Permission Groups tab:
   → Add: ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY (for Builders)
   OR
   → Add: ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY (for Explorers)
4. Verify the user's existing Payables security (Business Unit, Supplier access)
5. Save, assign to users
6. In AI Agent Studio → Agent Teams → Security tab: Add the custom role
```

---

### 📋 HCM Agents (HR / Workforce)

AI agents embedded in **Oracle HCM** such as HR Help Desk Agent, Onboarding Agent, and Employee Self-Service Agents.

#### Roles Required

| Tier | Role / Duty Name | Role Code | Tab |
|------|-----------------|----------|-----|
| Tier 2 | Manage HCM Intelligent Agent | `ORA_HRC_HCM_AI_AGENT_MANAGEMENT_DUTY` | Roles and Privileges |
| Tier 1 | Fai Genai Agent HCM Administrator Duty _(Builders)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_HCM_ADMINISTRATOR_DUTY` | Roles & Permission Groups |
| Tier 1 | Fai Genai Agent Runtime Duty _(Explorers)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY` | Roles & Permission Groups |

---

### 📦 SCM Agents (Supply Chain)

AI agents for procurement, order management, and supply chain operations.

#### Roles Required

| Tier | Role / Duty Name | Role Code | Tab |
|------|-----------------|----------|-----|
| Tier 1 | Fai Genai Agent SCM Administrator Duty _(Builders)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_SCM_ADMINISTRATOR_DUTY` | Roles & Permission Groups |
| Tier 1 | Fai Genai Agent Runtime Duty _(Explorers)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY` | Roles & Permission Groups |

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
│          for new agent-specific duty roles               │
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

When Oracle releases a new version (e.g., 26B → 27A), check these sources for new agent-specific roles:

| Source | How to Access |
|--------|---------------|
| **Oracle Cloud Readiness** | [cloud.oracle.com/readiness](https://www.oracle.com/cloud/readiness/) → Filter by Fusion Apps → AI / Agent Studio |
| **Oracle Help Center** | [docs.oracle.com/en/cloud/saas/fusion-ai](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/) |
| **Security Console → Role Search** | Search for new `ORA_DR_FAI_*` or `ORA_FAI_*` role codes after upgrade |
| **Oracle Security Reference Guides** | Available per product (FIN, HCM, SCM) on Oracle Help Center |
| **Oracle Support Notes** | Search [support.oracle.com](https://support.oracle.com) for agent security notes |

---

## 📈 Planned & Upcoming Oracle AI Agents (Reference)

The following types of AI agents are expected to be released by Oracle across future versions. Each will likely require similar role additions:

| Agent | Product Area | Expected Role Pattern |
|-------|--------------|-----------------------|
| Ledger Agent | Financials (GL) | Ledger Inquiry Assistant + FIN FAI duty |
| Payables Agent | Financials (AP) | FIN manage duties + FIN FAI duty |
| Receivables Agent | Financials (AR) | FIN manage duties + FIN FAI duty |
| Procurement Insights Agent | Procurement | PRC FAI Admin duty |
| Workforce Planning Agent | HCM | HCM manage duty + HCM FAI duty |
| Supply Chain Optimizer Agent | SCM | SCM FAI Admin duty |
| Service Request Agent | CRM/CX | CX FAI Admin duty |
| Permitting Agent | PSC | PSC FAI Admin duty |

> ⚠️ **Always verify the exact role codes** in Oracle documentation or Security Console after each upgrade, as role codes may differ from the pattern above.

---

## 📚 Official Reference Links

| Topic | Link |
|-------|------|
| Oracle Cloud Readiness (New Features) | [cloud.oracle.com/readiness](https://www.oracle.com/cloud/readiness/) |
| AI Agent Studio Documentation | [Oracle Docs – 26B](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/) |
| Access Requirements Overview | [Access AI Agent Studio](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-ai-agent-studio.html) |
| Access – All Products | [All Products Security](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-all-products.html) |
| Access – Financials | [Financials Security](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-financials.html) |
| Access – HCM | [HCM Security](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-human-capital-management-hcm.html) |
| Access – SCM | [SCM Security](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-supply-chain-manufacturing-scm.html) |

---

*Last updated: June 2026 | Oracle Fusion Cloud Release 26B*
