# 💬 Oracle AI Agent Studio – Explorer Role (AI Chat Access)

> **For End Users Who Access AI Agents via Chat — Not for Admins/Builders**

---

## 📖 Overview

In Oracle AI Agent Studio, there are **two distinct types of users**:

| User Type | What They Do | Role Type |
|-----------|-------------|----------|
| **Builder / Administrator** | Creates, configures, and manages AI agents in AI Agent Studio | Admin Duty Roles (covered in README.md) |
| **Explorer / End User** | Accesses and interacts with published AI agents via the **AI Chat** interface | Explorer / Runtime Roles (this guide) |

This guide covers how to provision the **Explorer Role** — enabling business users to chat with AI agents without needing access to AI Agent Studio itself.

---

## ✅ Explorer Role – Required Roles & Privileges

To give an end user access to interact with AI agents via AI Chat, you need to configure a custom job role with the following:

### Roles and Permission Groups Tab

| Role Name | Role Code |
|-----------|----------|
| Fai Genai Agent Runtime Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY` |

### Function Security Policies (Privileges)

| Privilege Name | Privilege Code |
|---------------|---------------|
| Access Intelligent Agent Chat | `HRC_ACCESS_AI_AGENT_CHAT_PRIV` |

> **⚠️ Note:** The `HRC_ACCESS_AI_AGENT_CHAT_PRIV` privilege is added under **Function Security Policies**, not the Role Hierarchy tab.

---

## 🔧 Step-by-Step: Set Up Explorer / Chat Access

### Step 1: Configure Profile Options (One-Time Setup)

Before the chat functionality works, two profile options must be enabled at the **Site level**:

| Profile Option | Profile Code | Required Value |
|---------------|-------------|---------------|
| Enable Security Console External Application Integration | `ORA_ASE_SAS_INTEGRATION_ENABLED` | **Yes** |
| Enable VBCS Progressive Web Application User Interface | `ORA_HCM_VBCS_PWA_ENABLED` | **Yes** |

**Navigation:** Setup and Maintenance → Manage Administrator Profile Values → Search for each profile option → Set to **Yes** at Site level.

> For Guided Journey-based agent access, also enable: `ORA_PER_AGENT_TASK_TYPE_GUIDED_JOURNEYS_ENABLED` = **Yes**

---

### Step 2: Create a Custom Job Role for Explorers

1. Navigate to **Tools → Security Console**
2. Click **Roles → Create Role**
3. Fill in role details:
   - **Role Name:** e.g., `Custom AI Agent Explorer`
   - **Role Code:** e.g., `CUSTOM_AI_AGENT_EXPLORER`
   - **Role Category:** Job Roles
4. **✅ Enable Permission Groups** (critical — must be checked)

---

### Step 3: Add Function Security Privilege

1. Go to the **Function Security Policies** page of your custom role
2. Click **Add Privilege**
3. Search for and add: **`HRC_ACCESS_AI_AGENT_CHAT_PRIV`** (Access Intelligent Agent Chat)
4. Save

---

### Step 4: Add Runtime Duty Role

1. Go to the **Role Hierarchy** page
2. Open the **Roles and Permission Groups** tab
3. Click **Add Role**
4. Search for and add: **`ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY`** (Fai Genai Agent Runtime Duty)
5. Save

---

### Step 5: Save & Assign the Custom Role

1. Save and close the custom role
2. Navigate to **Users** in Security Console
3. Find the user and assign the custom Explorer role
4. Ask the user to log out and log back in

---

### Step 6: Assign Role to an Agent Team in AI Agent Studio

This is a **critical step** — the Explorer role must be linked to the correct Agent Team to control which agents the user can see.

1. Navigate to **Tools → AI Agent Studio**
2. Click on **Agent Teams** tab
3. Open the relevant **Agent Team**
4. Click the **Agent Team Settings** icon (⚙️)
5. Go to the **Security** tab
6. Two options are available:

   | Option | Description |
   |--------|-------------|
   | **For All Employees (Toggle ON)** | Every employee in the org gets access to this agent team — no individual role assignment needed |
   | **Role-Based Access (Toggle OFF)** | Click **+ Add** and select your custom Explorer role to restrict access to specific users |

7. Save the Agent Team settings

> 💡 **Recommended:** Use Role-Based Access (Toggle OFF + Add specific roles) for production environments to maintain proper governance.

---

## 🔄 Run Required Security Sync Processes

After creating roles and making changes, run these scheduled processes to sync security data:

```
1. Scheduled Processes → Schedule New Process
2. Run in order:
   a. Import Resource Application Security Data
   b. Import User and Role Application Security Data
```

This ensures the roles are visible in the Agent Team Security tab and reflect correctly for users.

---

## 📊 Explorer vs. Administrator – Role Comparison

| Capability | Explorer (End User) | Administrator (Builder) |
|-----------|--------------------|-----------------------|
| Access AI Chat to talk to agents | ✅ Yes | ✅ Yes |
| View published agent responses | ✅ Yes | ✅ Yes |
| Create / Edit AI Agents | ❌ No | ✅ Yes |
| Access AI Agent Studio interface | ❌ No | ✅ Yes |
| Configure agent prompts / nodes | ❌ No | ✅ Yes |
| Deploy / Publish agents | ❌ No | ✅ Yes |
| Manage Agent Teams | ❌ No | ✅ Yes |
| Key Role | `ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY` + `HRC_ACCESS_AI_AGENT_CHAT_PRIV` | FAI Administrator Duty Roles per product pillar |

---

## 🔐 Security Design Principles for Explorer Access

### 1. Data Security is Inherited
Oracle AI agents **automatically respect the RBAC guardrails** of the logged-in user. The agent will only expose data that the user already has access to in their underlying Oracle Fusion roles. There is no need to re-configure data security specifically for agent access.

### 2. Least Privilege
Give Explorer users only:
- `HRC_ACCESS_AI_AGENT_CHAT_PRIV` (to open the chat)
- `ORA_DR_FAI_GENERATIVE_AI_AGENT_RUNTIME_DUTY` (to run agents)

Do **not** give them Administrator duty roles unless they need to build/configure agents.

### 3. Agent Team Security Controls What Agents Are Visible
Just having the Explorer role doesn't give access to all agents — the Agent Team security settings control which specific agents the role can interact with.

---

## ⚠️ Common Issues

| Issue | Cause | Fix |
|-------|-------|-----|
| User can't see AI Chat option | `HRC_ACCESS_AI_AGENT_CHAT_PRIV` missing | Add the privilege to the custom role |
| Agent not appearing in chat | Role not added to Agent Team | Go to Agent Team → Security tab → Add role |
| Roles not showing in Agent Team LOV | Permission Groups not enabled | Re-create role with Permission Groups enabled; run security sync jobs |
| Profile options not saved | Site-level not selected | Ensure profile values are set at **Site** level, not just User level |
| Still no access after role assignment | Security data not synced | Run: *Import Resource Application Security Data* then *Import User and Role Application Security Data* |

---

## 📚 Official Reference

- [Access Requirements for AI Agent Studio](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-ai-agent-studio.html)
- [Oracle Fusion AI for HCM – Security Guide](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/)

---

*Last updated: June 2026 | Oracle Fusion Cloud Release 26B*
