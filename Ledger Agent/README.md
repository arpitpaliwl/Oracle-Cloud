# 🧾 Oracle Ledger Agent — AI-Powered Accounting Operations

> **Part of Oracle ERP Agents | Oracle Fusion Cloud Financials**

Ledger Agent is the new General Ledger experience within Oracle Fusion Cloud — transforming accounting operations from periodic, manual processes into an **agentic, natural-language-driven workflow**. It delivers continuous financial visibility, proactive monitoring, and context-aware inquiry — all without IT or reporting dependencies.

---

## 📺 Video Tutorials

| # | Title | Link |
|---|-------|-------|
| 1 | Getting Started with Ledger Agent | [▶️ Watch on YouTube](https://youtu.be/ZuYnCh5ecwQ) |
| 2 | Ledger Agent — Honest Take and Adoption Guide Deep Dive | [▶️ Watch on YouTube](https://youtu.be/z2hQ_A3Hcws) |

---

## 📌 Table of Contents

- [What is Ledger Agent?](#what-is-ledger-agent)
- [Prerequisites & Requirements](#prerequisites--requirements)
- [Key Capabilities](#key-capabilities)
- [Demo Scenarios Walkthrough](#demo-scenarios-walkthrough)
- [Adoption Journey](#adoption-journey)
- [Getting Started](#getting-started)
- [Upcoming Features — Roadmap](#upcoming-features--roadmap)
- [FAQ](#faq)
- [Community & Resources](#community--resources)

---

## What is Ledger Agent?

Ledger Agent redefines how accounting teams work with the General Ledger. Instead of relying on static reports, manual exception tracking, and spreadsheets, Ledger Agent brings:

| Traditional Approach | With Ledger Agent |
|---|---|
| Day-to-day exception tracking via reports & spreadsheets | Agent-driven proactive monitoring of indicators & exceptions |
| Manual cross-functional collaboration for reconciliation | Natural language inquiry with context-aware explanations |
| Time-consuming manual journal adjustments | Full agentic GL experience for inquiry, actions & corrections |
| Periodic review of business indicators | High-quality, continuous financial visibility |

> 💡 Ledger Agent is an **additive capability** — your existing journals, period close, and GL work areas remain unchanged. No Redwood migration required.

---

## Prerequisites & Requirements

To use Ledger Agent, you need the following:

- **Oracle Fusion Cloud Financials** subscription (Release 26B or later)
- Access to the **General Ledger**  within Oracle Fusion (New Workarea)
- Appropriate **user roles** assigned
- **Data quality** — high-quality financial inputs lead to clearer, more insightful AI explanations
- No additional software installation or IT configuration required
- No formal training needed — just ask questions in natural business language

---

## Key Capabilities

### 🔔 1. Proactive Monitoring

Configure monitoring prompts in plain language and let the agent surface insights when your conditions are triggered.

- Write prompts in everyday business language
- Pick from a **catalog of 75+ best-practice templates**
- Set frequency: **hourly → monthly**
- Set time-window: **D-5 to D+2**
- Prompts are **private by default** — accounting managers can share and assign to team members
- No technical limit on number of monitoring prompts (best practice: curate quarterly)

**Example prompts:**
```
Travel expenses by company MoM > 5%
Manual posted journals without attachment
Non-Zero Residual Balance in Allocation Source Accounts
Suspense Account has a remaining balance
```

---

### 💡 2. Insight Explanations

The agent continuously scans for anomalies and operational exceptions and surfaces prioritised insights.

- **AI-generated contextual summaries** per insight
- **Prioritised inbox**: High / Medium / Low
- Seeded daily insights for common accounting exceptions
- Continue any insight into a **full conversational analysis**
- Insights are categorised: Operational, Variance, etc.

---

### 💬 3. Natural Language Inquiry

Interact with your financial data using free-form conversation — no IT or reporting dependency.

- Free-form chat or **guided prompt templates**
- Context-aware: understands ledger, entity, and period
- **Drill-down** from summary → cost center → invoice → supplier
- Answers are just a prompt away

**Example queries:**
```
Compare 6200 Utilities for 100 CK Electronics Holding between Aug-25 Sep-25 show variance %
Show unapproved/unposted/error journals for Manual in USD for Jan-26
```

---

### 📊 4. Account Analysis

Interact with financial data conversationally and replace manual export-pivot-analyse workflows.

- Balance comparisons across **dimensions & time periods**
- Visual summaries and **supporting documentation inline**
- Replaces manual export → pivot → analyse workflows
- Convert spot analysis into a **permanent monitoring prompt**
- Delivers detailed breakdowns, visual summaries, and even supporting documentation — all through a conversational interface

---

## Demo Scenarios Walkthrough

The following four demo scenarios showcase the core capabilities of Ledger Agent in action.

---

### 🎬 Demo Scenario 1: Expense Analysis
**Capability demonstrated:** Natural Language Inquiry + Follow-up

In this scenario, a finance user queries travel and entertainment expenses using natural language. The agent understands context (ledger, entity, period) and returns a breakdown by cost center with variance analysis — no report required.

**Sample query:**
```
Flag travel and entertainment expense (Account 6050) entries over $5,000 for additional review
```

---

### 🎬 Demo Scenario 2: Accounting Exceptions
**Capability demonstrated:** Seeded Insights + Monitoring

This scenario shows how the agent proactively surfaces accounting process exceptions — such as journals unapproved for more than a week or AP liability variances — directly in the Insights inbox, ranked by priority.

**Sample monitoring prompt:**
```
Journals unapproved for more than a week
```

---

### 🎬 Demo Scenario 3: Foreign Currency Balance Monitoring
**Capability demonstrated:** Insight to Analysis

A monitoring prompt is configured to track EUR account balances exceeding a threshold. When triggered, the insight card shows a contextual summary. The user then continues into a full drill-down analysis — from ledger summary to individual journal entries.

**Sample monitoring prompt:**
```
Entered account balance in EUR greater than 5000 for account 21010 and company 3221
```

---

### 🎬 Demo Scenario 4: Intercompany Reconciliation
**Capability demonstrated:** Natural Language Analysis

The agent is used to investigate intercompany balance differences between two entities. The user asks natural language questions to identify mismatches, drill into the root cause, and understand the reconciliation gap — without opening a single report.

**Sample query:**
```
Balance between company 3221 and intercompany 3111 for account 21010 displaying company, intercompany and account for current period and prior period exceeding 4000
```

---

## Adoption Journey

Ledger Agent is designed for **function-by-function adoption** — no large or disruptive projects required.

| Function | Today's Mechanism | With Ledger Agent |
|---|---|---|
| Variance Analysis | Account analysis reports, Account Monitor | → Monitoring Prompts |
| Account Walkthroughs | Inquire & Analyze Balances, Interactive Reports | → Guided Prompts → explore details |
| Daily Accounting Ops | Account analysis / OTBI reports, Create Accounting | → Monitoring Prompts |
| Ad-hoc Inquiry | Online Inquiry, Saved Searches, SmartView | → Natural Language + Guided Prompts |
| Operational Checks | Account analysis, Journal reports with conditions | → Monitoring Prompts |

---

## Getting Started

### Adoption Principles suggested by Oracle
1. Ledger Agent is **additive** — low/no risk to existing processes which we do currently
2. Use a **progressive, function-by-function methodology**
3. No large projects — embrace new automation as part of regular activities

### Recommended First Steps
1. **Start with Monitoring** — Review the catalog of monitoring prompts and pick the ones most relevant to your role
2. Tailor the prompts to your ledger, entity, and accounts for accuracy and control
3. Establish Ledger Agent as the **preferred front door** for all ad-hoc GL inquiries
4. Gradually replace manual reporting workflows with guided prompts and natural language inquiry

> 💡 **Pro Tip:** Your data quality matters. High-quality financial inputs lead to clearer, more insightful AI explanations.

---

## Upcoming Features — Roadmap

| # | Feature | Description |
|---|---------|-------------|
| 01 | **Connected GL Actions** | Create reclassification journals, adjustments, and accounting corrections directly from an insight or analysis thread — without leaving the agent |
| 02 | **Reconciliation Differences** | Automatically monitor and explain differences between Ledger and subledger balances, clearing accounts, and intercompany accounts |
| 03 | **Allocations** | Author and preview allocation rules using natural language through the agent |
| 04 | **Financial Statements with Narratives** | View financial reports directly in the agent, enriched with AI-generated narratives and variance explanations |
| 05 | **SmartView Integration** | Launch further analysis directly into SmartView from Ledger Agent, combining NL inquiry with familiar pivot analytics |

---

## FAQ

**Q: Does Ledger Agent replace our existing GL pages?**  
No — Ledger Agent is an add-on. Existing journals, period close, and GL work areas remain unchanged. No Redwood migration is needed.

**Q: Can we create journals from the agent?**  
Journal creation (reclassifications, adjustments) is a key upcoming feature. Connected GL Actions is a top priority on the roadmap.

**Q: Who can see monitoring prompts?**  
Prompts are private by default. Accounting managers can share and assign prompts to team members for consistent organisational coverage.

**Q: Is there a limit to monitoring prompts?**  
No technical limit. Best practice: regularly curate and disable stale prompts as business conditions change each quarter.

**Q: Does it support fixed assets and subledgers?**  
Natural language interaction works across subledgers. Deep subledger NL responses (e.g. fixed assets) will expand in subsequent releases.

**Q: Is training required?**  
No formal training needed. Just ask the questions you already ask yourself daily — the agent understands natural business language.

**Q: What is the Redirect/Callback URI for Google OAuth setup?**  
Use the following format: `https://<podurl>/api/rwdinfra/trap/2/oauth/callback`  
Add this as the required Redirect URI in your Google OAuth configuration.

---

## Community & Resources

| Resource | Link |
|---|---|
| 📖 Oracle Fusion Cloud Financials — What's New (26B) | [View Documentation](https://docs.oracle.com/en/cloud/saas/financials/26b/whatsnew/) |
| 💬 Oracle Cloud Customer Connect — Ledger Agent Community | [Join the Community](https://community.oracle.com/customerconnect/categories/erp-ledger-agent) |
| 📋 Ledger Agent Adoption Guidance (Oracle Doc) | Available via Oracle Help Center |
| 🎬 YouTube Tutorial 1 — Getting Started with Ledger Agent | [Watch Here](https://youtu.be/ZuYnCh5ecwQ) |
| 🎬 YouTube Tutorial 2 — Honest Take and AAdoption Guide Deep Dive | [Watch Here](https://youtu.be/z2hQ_A3Hcws) |

> 💬 Have questions or suggestions? Feel free to open an **Issue** or start a **Discussion** in this repository!

---

*This repository is maintained as a companion resource to the YouTube video series on Oracle ERP Agents — Ledger Agent.*
