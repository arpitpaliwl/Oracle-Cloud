# 🔐 Step-by-Step Security Setup Guide

> Oracle AI Agent Studio – Detailed Configuration Instructions

---

## Overview

This guide provides detailed, step-by-step instructions for setting up security access to Oracle AI Agent Studio in Oracle Fusion Cloud.

> **Note:** These steps must be performed by a user with the **IT Security Manager** role or equivalent administrative privileges in the Oracle Security Console.

---

## Step 1: Open Oracle Security Console

1. Log in to **Oracle Fusion Cloud**
2. Navigate to **Tools** in the top navigation bar
3. Click on **Security Console**
4. The Security Console home page opens

---

## Step 2: Enable Permission Groups

> This is a **critical prerequisite**. Without enabling Permission Groups, the **Roles and Permission Groups** tab will not be visible.

1. In Security Console, click on the **Administration** menu
2. Navigate to **Role Configuration** or **Preferences**
3. Ensure the **Enable Permission Groups** setting is turned ON
4. Save changes if required

---

## Step 3: Create a New Custom Job Role

1. In Security Console, click on **Roles**
2. Click the **+ Create Role** button
3. Fill in the role details:
   - **Role Name:** e.g., `Custom AI Agent Studio Administrator`
   - **Role Code:** e.g., `CUSTOM_AI_AGENT_STUDIO_ADMIN`
   - **Role Category:** Select appropriate category (e.g., Job Roles)
4. Click **Next** to proceed to Role Hierarchy

> ⚠️ **Tip:** You can also add AI Agent Studio duty roles to an **existing** custom job role instead of creating a new one.

---

## Step 4: Add Roles on Roles and Privileges Tab

1. On the **Role Hierarchy** page, click the **Roles and Privileges** tab
2. Click **Add Role**
3. Search for and add the product-specific duty roles:

   **For HCM:**
   - `ORA_HRC_HCM_AI_AGENT_MANAGEMENT_DUTY` (Manage HCM Intelligent Agent)

   **For Financials:**
   - `ORA_FUN_MANAGE_FIN_AI_AGENT_HCM` (Manage Financials Intelligent Agent - HCM)
   - `ORA_FUN_MANAGE_FIN_AI_AGENT` (Manage Financials Intelligent Agent)

   **For All Products (Super Admin):**
   - `ORA_FAI_MANAGE_ALL_AI_AGENTS` (Manage All Intelligent Agents)

---

## Step 5: Add Roles on Roles and Permission Groups Tab

1. Click the **Roles and Permission Groups** tab
2. Click **Add Role** or **Add Permission Group**
3. Search for and add the FAI Administrator Duty roles for your product area:

   | Product | Role Code |
   |---------|----------|
   | CX | `ORA_DR_FAI_GENERATIVE_AI_AGENT_CX_ADMINISTRATOR_DUTY` |
   | FIN | `ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY` |
   | GRC | `ORA_DR_FAI_GENERATIVE_AI_AGENT_GRC_ADMINISTRATOR_DUTY` |
   | HCM | `ORA_DR_FAI_GENERATIVE_AI_AGENT_HCM_ADMINISTRATOR_DUTY` |
   | PRC | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PRC_ADMINISTRATOR_DUTY` |
   | PRJ | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PRJ_ADMINISTRATOR_DUTY` |
   | PSC | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PSC_ADMINISTRATOR_DUTY` |
   | SCM | `ORA_DR_FAI_GENERATIVE_AI_AGENT_SCM_ADMINISTRATOR_DUTY` |

---

## Step 6: Save and Review the Custom Role

1. Click **Next** through any remaining wizard steps
2. On the **Summary** page, review all assigned roles
3. Click **Save and Close**
4. The custom role is now created with the required AI Agent Studio permissions

---

## Step 7: Assign the Role to Users

1. In Security Console, navigate to **Users**
2. Search for the user who needs access
3. Click on the user to open their profile
4. Click **Edit**
5. Go to the **Roles** tab
6. Click **Add Role**
7. Search for and select your custom job role
8. Click **Save and Close**

---

## Step 8: Verify Access

1. Ask the user to **log out** and **log back in** to Oracle Fusion Cloud
2. Navigate to **AI Agent Studio** using the Navigator or Tools menu
3. Confirm the user can:
   - See the AI Agent Studio interface
   - View existing agents
   - Create/edit agents (if the appropriate admin privileges are assigned)

---

## Troubleshooting

### User cannot see AI Agent Studio
- Verify all required duty roles are assigned in **both** the Roles and Privileges tab AND the Roles and Permission Groups tab
- Ensure Permission Groups are enabled
- Check that the custom role is saved and assigned to the user

### Roles and Permission Groups tab not visible
- Enable Permission Groups in Security Console settings

### Changes not taking effect
- Have the user log out and log back in
- In some cases, it may take a few minutes for role changes to propagate

### User can see Studio but cannot create agents
- Ensure `ORA_FAI_MANAGE_ALL_AI_AGENTS` is added to the Roles and Privileges tab

---

## Official Documentation

- [Access Requirements for AI Agent Studio](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-ai-agent-studio.html)
- [Access to Configure AI Agents in All Products](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-all-products.html)
- [HCM Security Configuration](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-human-capital-management-hcm.html)
- [SCM Security Configuration](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-supply-chain-manufacturing-scm.html)
- [PRC Security Configuration](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-procurement-prc.html)
- [FIN Security Configuration](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-financials.html)
- [CRM/CX Security Configuration](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-to-configure-ai-agents-in-oracle-fusion-cloud-crm.html)
- [PSC Security Configuration](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/provide-access-to-configure-ai-agents-in-oracle-permitting-and-licensing.html)
