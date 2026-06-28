# 🔐 Security Roles Quick Reference Card

> Oracle AI Agent Studio – All Duty Roles at a Glance

---

## Roles & Permission Groups Tab – FAI Administrator Duty Roles

| Product | Duty Role Name | Duty Role Code |
|---------|---------------|----------------|
| CX (CRM) | Fai Genai Agent CX Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_CX_ADMINISTRATOR_DUTY` |
| FIN (Financials) | Fai Genai Agent FIN Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY` |
| GRC (Governance, Risk & Compliance) | Fai Genai Agent GRC Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_GRC_ADMINISTRATOR_DUTY` |
| HCM (Human Capital Management) | Fai Genai Agent HCM Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_HCM_ADMINISTRATOR_DUTY` |
| PRC (Procurement) | Fai Genai Agent PRC Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PRC_ADMINISTRATOR_DUTY` |
| PRJ (Project Management) | Fai Genai Agent PRJ Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PRJ_ADMINISTRATOR_DUTY` |
| PSC (Permitting & Licensing) | Fai Genai Agent PSC Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PSC_ADMINISTRATOR_DUTY` |
| SCM (Supply Chain & Mfg.) | Fai Genai Agent SCM Administrator Duty | `ORA_DR_FAI_GENERATIVE_AI_AGENT_SCM_ADMINISTRATOR_DUTY` |

---

## Roles & Privileges Tab – Product-Specific Duty Roles

| Product | Duty Role Name | Duty Role Code |
|---------|---------------|----------------|
| HCM | Manage HCM Intelligent Agent | `ORA_HRC_HCM_AI_AGENT_MANAGEMENT_DUTY` |
| FIN | Manage Financials Intelligent Agent | `ORA_FUN_MANAGE_FIN_AI_AGENT` |
| FIN | Manage Financials Intelligent Agent (HCM) | `ORA_FUN_MANAGE_FIN_AI_AGENT_HCM` |
| All Products | Manage All Intelligent Agents | `ORA_FAI_MANAGE_ALL_AI_AGENTS` |

---

## Minimum Access Matrix

| User Type | Minimum Roles Needed |
|-----------|---------------------|
| HCM AI Agent Admin | `ORA_HRC_HCM_AI_AGENT_MANAGEMENT_DUTY` + `ORA_DR_FAI_GENERATIVE_AI_AGENT_HCM_ADMINISTRATOR_DUTY` |
| FIN AI Agent Admin | `ORA_FUN_MANAGE_FIN_AI_AGENT` + `ORA_FUN_MANAGE_FIN_AI_AGENT_HCM` + `ORA_DR_FAI_GENERATIVE_AI_AGENT_FIN_ADMINISTRATOR_DUTY` |
| SCM AI Agent Admin | `ORA_DR_FAI_GENERATIVE_AI_AGENT_SCM_ADMINISTRATOR_DUTY` |
| PRC AI Agent Admin | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PRC_ADMINISTRATOR_DUTY` |
| CX AI Agent Admin | `ORA_DR_FAI_GENERATIVE_AI_AGENT_CX_ADMINISTRATOR_DUTY` |
| PSC AI Agent Admin | `ORA_DR_FAI_GENERATIVE_AI_AGENT_PSC_ADMINISTRATOR_DUTY` |
| Super Admin (All) | `ORA_FAI_MANAGE_ALL_AI_AGENTS` + All 8 FAI Administrator Duty roles |

---

## Key Checklist Before Raising Support Ticket

- [ ] Permission Groups are enabled in Security Console
- [ ] Custom Job Role has been created
- [ ] Correct duty roles added to **Roles and Privileges** tab
- [ ] Correct duty roles added to **Roles and Permission Groups** tab
- [ ] Custom job role is saved
- [ ] Custom job role is assigned to the user
- [ ] User has refreshed their browser / logged out and back in
- [ ] Generative AI feature flags are enabled in the Fusion instance
