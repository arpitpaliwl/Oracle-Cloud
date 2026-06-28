# 🔐 Oracle AI Agent Studio – Security & Access Configuration Guide

> **Complete Role-Based Access Configuration for Oracle Fusion Cloud AI Agent Studio across all Product Pillars**

[![Oracle Fusion Cloud](https://img.shields.io/badge/Oracle%20Fusion%20Cloud-26B-red?style=flat-square)](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/)
[![Release](https://img.shields.io/badge/Release-26B-orange?style=flat-square)](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/)
[![License](https://img.shields.io/badge/License-UPL--1.0-blue?style=flat-square)](https://opensource.org/licenses/UPL)

---

## 📖 Overview

Oracle AI Agent Studio is a powerful no-code / low-code platform that allows you to design, configure, and deploy AI-powered workflow agents directly within Oracle Fusion Cloud.

Before you can build and manage AI agents, you **must configure the appropriate security roles and permissions** in the Oracle Security Console. Incorrect or missing role assignments are the #1 reason why users cannot access AI Agent Studio.

This guide covers:
- ✅ How to provide access to AI Agent Studio
- ✅ Prerequisites and dependencies
- ✅ Security roles for **all product pillars** (HCM, SCM, PRC, FIN, CRM/CX, PSC)
- ✅ Step-by-step configuration instructions
- ✅ Quick Reference Role Tables

---

## 📂 Files in This Folder

| File | Description |
|------|-------------|
| **README.md** *(this file)* | Main Security Guide – Admin & Builder roles for all product pillars |
| [ExplorerRole-ChatAccess.md](./ExplorerRole-ChatAccess.md) | 💬 Explorer / End-User role for accessing agents via AI Chat |
| [NewAgentFunctionality-RoleAssignment.md](./NewAgentFunctionality-RoleAssignment.md) | ➕ How to add roles for new agents (Ledger Agent, Payables Agent, etc.) |
| [SecurityRolesQuickReference.md](./SecurityRolesQuickReference.md) | 📊 All role codes in one quick-lookup table + checklist |
| [StepByStepSetupGuide.md](./StepByStepSetupGuide.md) | 🔧 Step-by-step walkthrough from Security Console to user assignment |
| [SecurityArchitecture.md](./SecurityArchitecture.md) | 🏗️ Architecture diagrams, two-tab security model, best practices |

---

## 📋 Table of Contents

- [Prerequisites](#-prerequisites)
- [How to Access AI Agent Studio](#-how-to-access-ai-agent-studio)
- [Access for All Products (Super Admin)](#-access-to-configure-ai-agents-in-all-products)
- [Product-Specific Access Configuration](#-product-specific-access-configuration)
  - [Human Capital Management (HCM)](#1️⃣-oracle-fusion-cloud-human-capital-management-hcm)
  - [Supply Chain & Manufacturing (SCM)](#2️⃣-oracle-fusion-cloud-supply-chain--manufacturing-scm)
  - [Procurement (PRC)](#3️⃣-oracle-fusion-cloud-procurement-prc)
  - [Financials (FIN)](#4️⃣-oracle-fusion-cloud-financials-fin)
  - [CRM / Customer Experience (CX)](#5️⃣-oracle-fusion-cloud-crm--customer-experience-cx)
  - [Permitting & Licensing (PSC)](#6️⃣-oracle-permitting-and-licensing-psc)
- [Explorer Role – Chat Access](#-explorer-role--chat-access)
- [Adding New Agent Roles](#-adding-new-agent-functionality-roles)
- [Complete Duty Roles Reference Table](#-complete-duty-roles-reference-table)
- [Common Issues & Troubleshooting](#-common-issues--troubleshooting)
- [Official Documentation Links](#-official-documentation-links)

---

## ✅ Prerequisites

Before configuring access to AI Agent Studio, ensure the following are completed:

| # | Prerequisite | Details |
|---|-------------|----------|
| 1 | **Oracle Fusion Cloud Instance** | Access to an active Oracle Fusion Cloud instance (26B or later) |
| 2 | **Security Console Access** | You must have the IT Security Manager role or equivalent to configure security in the Security Console |
| 3 | **Permission Groups Enabled** | Permission Groups must be enabled in the Security Console |
| 4 | **Profile Options Set** | `ORA_ASE_SAS_INTEGRATION_ENABLED` = Yes and `ORA_HCM_VBCS_PWA_ENABLED` = Yes at Site level |
| 5 | **Generative AI Feature Enabled** | Ensure that Oracle Generative AI features are enabled in your cloud instance |
| 6 | **Valid Oracle Subscription** | AI Agent Studio requires the appropriate Oracle cloud subscription |

> **⚠️ Important:** You must enable **Permission Groups** in the Security Console before adding duty roles. Without enabling this setting, the Roles and Permission Groups tab will not be visible on the Role Hierarchy page.

---

## 🚪 How to Access AI Agent Studio

You can give access to AI Agent Studio by assigning predefined **duty roles** to **job roles** using the Oracle Security Console.

### Step-by-Step: Access AI Agent Studio

```
1. Navigate to: Tools → Security Console
2. Create a new Custom Job Role (or use an existing one)
3. Enable Permission Groups in the Security Console settings
4. On the Role Hierarchy page:
   a. Open the "Roles and Privileges" tab → Add relevant duty roles
   b. Open the "Roles and Permission Groups" tab → Add FAI duty roles
5. Save the custom role
6. Assign the role to the appropriate users
```

### Navigation to AI Agent Studio

Once access is configured, users can navigate to AI Agent Studio via:

```
Oracle Fusion Cloud Home → AI Agent Studio
```

or using the Navigator menu in Oracle Fusion Cloud applications.

📚 **Official Reference:** [Access Requirements for AI Agent Studio](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-ai-agent-studio.html)

---

## 🌐 Access to Configure AI Agents in All Products

If you want a **single administrator** to configure AI agents across **all Oracle Fusion Cloud products**, create a custom job role with the following comprehensive duty roles.

### Step-by-Step Configuration

**Step 1:** Go to **Security Console** → Create a new custom job role

**Step 2:** Enable Permission Groups

**Step 3:** On the Role Hierarchy page → Open **"Roles and Permission Groups"** tab → Add ALL of the following duty roles:

| Duty Role Name | Role Code |
|---------------|----------|
| Fai Genai Agent CX Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_CX_ADMINISTRATOR_DUTY` |
| Fai Genai Agent FIN Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY` |
| Fai Genai Agent GRC Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_GRC_ADMINISTRATOR_DUTY` |
| Fai Genai Agent HCM Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_HCM_ADMINISTRATOR_DUTY` |
| Fai Genai Agent PRC Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PRC_ADMINISTRATOR_DUTY` |
| Fai Genai Agent PRJ Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PRJ_ADMINISTRATOR_DUTY` |
| Fai Genai Agent PSC Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PSC_ADMINISTRATOR_DUTY` |
| Fai Genai Agent SCM Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_SCM_ADMINISTRATOR_DUTY` |

**Step 4:** Open the **"Roles and Privileges"** tab → Add the following privilege:

| Privilege Name | Privilege Code |
|--------------|---------------|
| Manage All Intelligent Agents | `ORA_FAI_MANAGE_ALL_AI_AGENTS` |

**Step 5:** Save the custom role and assign it to users who need full access.

📚 **Official Reference:** [Access to Configure AI Agents in All Products](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-all-products.html)

---

## 🎯 Product-Specific Access Configuration

### 1️⃣ Oracle Fusion Cloud Human Capital Management (HCM)

For users who need to configure AI agents within the **HCM** product area.

#### Roles Required

| Tab | Role/Privilege Name | Role Code |
|----|--------------------|-----------|
| Roles and Privileges | Manage HCM Intelligent Agent (Duty Role) | `ORA_HRC_HCM_AI_AGENT_MANAGEMENT_DUTY` |
| Roles and Permission Groups | Fai Genai Agent HCM Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_HCM_ADMINISTRATOR_DUTY` |

#### Configuration Steps

```
1. Go to Security Console → Create a new custom job role
2. Enable Permission Groups
3. Navigate to Role Hierarchy page
4. Open "Roles and Privileges" tab:
   → Add: Manage HCM Intelligent Agent (ORA_HRC_HCM_AI_AGENT_MANAGEMENT_DUTY)
5. Open "Roles and Permission Groups" tab:
   → Add: Fai Genai Agent HCM Administrator Duty (ORA_DR_FAI_GENERATIVE_AI_AGENT_HCM_ADMINISTRATOR_DUTY)
6. Save the custom role
7. Assign to appropriate users / job roles
```

📚 **Official Reference:** [Access to Configure AI Agents in HCM](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-human-capital-management-hcm.html)

---

### 2️⃣ Oracle Fusion Cloud Supply Chain & Manufacturing (SCM)

For users who need to configure AI agents within the **SCM** product area.

#### Roles Required

| Tab | Role/Privilege Name | Role Code |
|----|--------------------|-----------|
| Roles and Permission Groups | Fai Genai Agent SCM Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_SCM_ADMINISTRATOR_DUTY` |

#### Configuration Steps

```
1. Go to Security Console → Create a new custom job role
2. Enable Permission Groups
3. Navigate to Role Hierarchy page
4. Open "Roles and Permission Groups" tab:
   → Add: Fai Genai Agent SCM Administrator Duty (ORA_DR_FAI_GENERATIVE_AI_AGENT_SCM_ADMINISTRATOR_DUTY)
5. Save the custom role
6. Assign to appropriate users / job roles
```

📚 **Official Reference:** [Access to Configure AI Agents in SCM](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-supply-chain-manufacturing-scm.html)

---

### 3️⃣ Oracle Fusion Cloud Procurement (PRC)

For users who need to configure AI agents within the **Procurement** product area.

#### Roles Required

| Tab | Role/Privilege Name | Role Code |
|----|--------------------|-----------|
| Roles and Permission Groups | Fai Genai Agent PRC Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PRC_ADMINISTRATOR_DUTY` |

#### Configuration Steps

```
1. Go to Security Console → Create a new custom job role
2. Enable Permission Groups
3. Navigate to Role Hierarchy page
4. Open "Roles and Permission Groups" tab:
   → Add: Fai Genai Agent PRC Administrator Duty (ORA_DR_FAI_GENERATIVE_AI_AGENT_PRC_ADMINISTRATOR_DUTY)
5. Save the custom role
6. Assign to appropriate users / job roles
```

📚 **Official Reference:** [Access to Configure AI Agents in Procurement](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-procurement-prc.html)

---

### 4️⃣ Oracle Fusion Cloud Financials (FIN)

For users who need to configure AI agents within the **Financials** product area.

#### Roles Required

| Tab | Role/Privilege Name | Role Code |
|----|--------------------|-----------|
| Roles and Privileges | Manage Financials Intelligent Agent (HCM) | `ORA_FUN_MANAGE_FIN_AI_AGENT_HCM` |
| Roles and Privileges | Manage Financials Intelligent Agent | `ORA_FUN_MANAGE_FIN_AI_AGENT` |
| Roles and Permission Groups | Fai Genai Agent FIN Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY` |

#### Configuration Steps

```
1. Go to Security Console → Create a new custom job role
2. Enable Permission Groups
3. Navigate to Role Hierarchy page
4. Open "Roles and Privileges" tab:
   → Add: Manage Financials Intelligent Agent (ORA_FUN_MANAGE_FIN_AI_AGENT_HCM)
   → Add: Manage Financials Intelligent Agent (ORA_FUN_MANAGE_FIN_AI_AGENT)
5. Open "Roles and Permission Groups" tab:
   → Add: Fai Genai Agent FIN Administrator Duty (ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY)
6. Save the custom role
7. Assign to appropriate users / job roles
```

📚 **Official Reference:** [Access to Configure AI Agents in Financials](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-financials.html)

---

### 5️⃣ Oracle Fusion Cloud CRM / Customer Experience (CX)

For users who need to configure AI agents within the **CRM / CX** product area.

#### Roles Required

| Tab | Role/Privilege Name | Role Code |
|----|--------------------|-----------|
| Roles and Permission Groups | Fai Genai Agent CX Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_CX_ADMINISTRATOR_DUTY` |

#### Configuration Steps

```
1. Go to Security Console → Create a new custom job role
2. Enable Permission Groups
3. Navigate to Role Hierarchy page
4. Open "Roles and Permission Groups" tab:
   → Add: Fai Genai Agent CX Administrator Duty (ORA_DR_FAI_GENERATIVE_AI_AGENT_CX_ADMINISTRATOR_DUTY)
5. Save the custom role
6. Assign to appropriate users / job roles
```

📚 **Official Reference:** [Access to Configure AI Agents in CRM/CX](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-crm.html)

---

### 6️⃣ Oracle Permitting and Licensing (PSC)

For users who need to configure AI agents within the **Oracle Permitting and Licensing** product area.

#### Roles Required

| Tab | Role/Privilege Name | Role Code |
|----|--------------------|-----------|
| Roles and Permission Groups | Fai Genai Agent PSC Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PSC_ADMINISTRATOR_DUTY` |

#### Configuration Steps

```
1. Go to Security Console → Create a new custom job role
2. Enable Permission Groups
3. Navigate to Role Hierarchy page
4. Open "Roles and Permission Groups" tab:
   → Add: Fai Genai Agent PSC Administrator Duty (ORA_DR_FAI_GENERATIVE_AI_AGENT_PSC_ADMINISTRATOR_DUTY)
5. Save the custom role
6. Assign to appropriate users / job roles
```

📚 **Official Reference:** [Access to Configure AI Agents in Permitting and Licensing](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/provide-access-to-configure-ai-agents-in-oracle-permitting-and-licensing.html)

---

## 💬 Explorer Role – Chat Access

For **end users** who need to access and interact with published AI agents via the **AI Chat** interface (not configure/build agents), a lighter-weight **Explorer Role** is used.

| Requirement | Value |
|------------|-------|
| Function Security Privilege | `HRC_ACCESS_AI_AGENT_CHAT_PRIV` (Access Intelligent Agent Chat) |
| Permission Group | `ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY` (Fai Genai Agent Runtime Duty) |
| Profile Options Required | `ORA_ASE_SAS_INTEGRATION_ENABLED` = Yes, `ORA_HCM_VBCS_PWA_ENABLED` = Yes |

📄 **See full details:** [ExplorerRole-ChatAccess.md](./ExplorerRole-ChatAccess.md)

---

## ➕ Adding New Agent Functionality Roles

When Oracle releases new AI agents (e.g., **Ledger Agent**, **Payables Agent**), additional product-specific roles must be added to user's custom job roles.

| Agent | Additional Roles Needed |
|-------|------------------------|
| **Ledger Agent** (GL) | Ledger Inquiry Assistant duty role + Data Access Set |
| **Payables Agent** (AP) | `ORA_FUN_MANAGE_FIN_AI_AGENT` + `ORA_FUN_MANAGE_FIN_AI_AGENT_HCM` |
| **HCM Agents** | `ORA_HRC_HCM_AI_AGENT_MANAGEMENT_DUTY` |
| **SCM Agents** | FAI SCM Administrator Duty |

📄 **See full details & repeatable process:** [NewAgentFunctionality-RoleAssignment.md](./NewAgentFunctionality-RoleAssignment.md)

---

## 📊 Complete Duty Roles Reference Table

| Product Pillar | Roles & Privileges Tab (Duty Roles) | Roles & Permission Groups Tab (FAI Duty Roles) |
|---------------|-----------------------------------|-------------------------------------------------|
| **All Products** | `ORA_FAI_MANAGE_ALL_AI_AGENTS` | CX, FIN, GRC, HCM, PRC, PRJ, PSC, SCM Administrator Duties |
| **HCM** | `ORA_HRC_HCM_AI_AGENT_MANAGEMENT_DUTY` | `ORA_DR_FAI_GENERATIVE_AI_AGENT_HCM_ADMINISTRATOR_DUTY` |
| **SCM** | _(None specific – use All Products for SCM privilege)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_SCM_ADMINISTRATOR_DUTY` |
| **PRC** | _(None specific – use All Products for PRC privilege)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PRC_ADMINISTRATOR_DUTY` |
| **FIN** | `ORA_FUN_MANAGE_FIN_AI_AGENT_HCM` + `ORA_FUN_MANAGE_FIN_AI_AGENT` | `ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY` |
| **CRM / CX** | _(None specific – use All Products for CX privilege)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_CX_ADMINISTRATOR_DUTY` |
| **PSC** | _(None specific – use All Products for PSC privilege)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PSC_ADMINISTRATOR_DUTY` |
| **GRC** | _(None specific – use All Products for GRC privilege)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_GRC_ADMINISTRATOR_DUTY` |
| **PRJ** | _(None specific – use All Products for PRJ privilege)_ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PRJ_ADMINISTRATOR_DUTY` |
| **Explorer (Chat)** | `HRC_ACCESS_AI_AGENT_CHAT_PRIV` (Function Security) | `ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY` |

---

## 🔧 Common Issues & Troubleshooting

| Issue | Root Cause | Resolution |
|-------|-----------|------------|
| Can't see AI Agent Studio in menu | Missing duty roles on job role | Add the FAI Administrator Duty roles for the required product pillars |
| "Roles and Permission Groups" tab not visible | Permission Groups not enabled | Go to Security Console → Enable Permission Groups |
| Can't create or edit agents | Missing `Manage All Intelligent Agents` privilege | Add `ORA_FAI_MANAGE_ALL_AI_AGENTS` to the Roles and Privileges tab |
| Access to HCM agents denied | Missing `ORA_HRC_HCM_AI_AGENT_MANAGEMENT_DUTY` | Add the HCM Intelligent Agent duty role to Roles and Privileges tab |
| Access to FIN agents denied | Missing one or both FIN duty roles | Add both `ORA_FUN_MANAGE_FIN_AI_AGENT_HCM` and `ORA_FUN_MANAGE_FIN_AI_AGENT` |
| Explorer user can't open AI Chat | Missing `HRC_ACCESS_AI_AGENT_CHAT_PRIV` | Add the privilege under Function Security Policies |
| Agent not visible in chat | Role not added to Agent Team | Go to AI Agent Studio → Agent Teams → Security tab → Add role |
| Changes not reflecting for user | Role not assigned to user | Navigate to Users → Find User → Assign the custom job role |
| Roles missing in Agent Team LOV | Security sync not run | Run: Import Resource Security Data → Import User/Role Security Data |

---

## 📚 Official Documentation Links

| Topic | Official Oracle Docs |
|-------|---------------------|
| Access Requirements for AI Agent Studio | [🔗 Link](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-ai-agent-studio.html) |
| Access to Configure AI Agents in All Products | [🔗 Link](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-all-products.html) |
| Access to Configure AI Agents – HCM | [🔗 Link](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-human-capital-management-hcm.html) |
| Access to Configure AI Agents – SCM | [🔗 Link](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-supply-chain-manufacturing-scm.html) |
| Access to Configure AI Agents – PRC | [🔗 Link](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-procurement-prc.html) |
| Access to Configure AI Agents – FIN | [🔗 Link](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-financials.html) |
| Access to Configure AI Agents – CRM/CX | [🔗 Link](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-crm.html) |
| Access to Configure AI Agents – PSC | [🔗 Link](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/provide-access-to-configure-ai-agents-in-oracle-permitting-and-licensing.html) |

---

## 🤝 Contributing

Found an error or have improvements to suggest?

- 📌 Open an [Issue](https://github.com/arpitpaliwl/Oracle-Cloud/issues)
- 🔁 Submit a Pull Request
- 💬 Connect on [LinkedIn – Arpit Paliwal](https://www.linkedin.com/in/arpitpaliwal/)

---

## 📺 Part of the Oracle AI Agent Studio YouTube Series

This guide is part of the **Oracle AI Agent Studio – Workflow Agents YouTube Series**.

Subscribe and follow along as we build real-world AI agents for Oracle Fusion Cloud:
- Episode 1: Introduction to Oracle AI Agent Studio
- Episode 2: Core Concepts – Nodes, LLMs & Prompts
- Episode 3: Building Your First Workflow Agent
- Episode 4: Logic Nodes, Data Nodes & Advanced Flows
- Episode 5: Security Configuration *(This Guide)*
- Episode 6: Use Case 1 – AP Invoice Processor
- Episode 7: Use Case 2 – Advanced Workflow

---

## 📄 License

Copyright (c) 2026 Arpit Paliwal

Licensed under the Universal Permissive License v 1.0 as shown at https://oss.oracle.com/licenses/upl

---

*Last updated: June 2026 | Oracle Fusion Cloud Release 26B*
