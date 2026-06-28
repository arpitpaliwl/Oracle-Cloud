# 🏗️ Security Architecture Overview

> Oracle AI Agent Studio – Security Model & Role Hierarchy

---

## How Oracle AI Agent Studio Security Works

Oracle AI Agent Studio uses **Role-Based Access Control (RBAC)** built on top of the standard Oracle Fusion Cloud security model. Access is controlled through a two-layer role system:

```
┌──────────────────────────────────────────┐
│           USER ACCOUNT                    │
└───────────────────┬─────────────────────┘
                        │
                        ▼
┌──────────────────────────────────────────┐
│         CUSTOM JOB ROLE                   │
│  (Created in Oracle Security Console)     │
└──────┬─────────────────────┬───────────────┘
               │                       │
               ▼                       ▼
┌──────────────────┐  ┌───────────────────┐
│  ROLES &          │  │  ROLES &           │
│  PRIVILEGES       │  │  PERMISSION GROUPS │
│  (Product-Specific│  │  (FAI Admin Duties)│
│   Duty Roles)     │  │                   │
└──────────────────┘  └───────────────────┘
```

---

## Two Types of Role Assignments

### 1. Roles and Privileges Tab
This tab contains **product-specific** duty roles that grant access to manage intelligent agents within a specific Oracle Fusion module.

- These are traditional Oracle Fusion security duty roles
- They control what data and operations the user can access
- Examples: `ORA_HRC_HCM_AI_AGENT_MANAGEMENT_DUTY`, `ORA_FUN_MANAGE_FIN_AI_AGENT`

### 2. Roles and Permission Groups Tab
This tab contains **Oracle AI (FAI) Administrator Duty Roles** that control access to the AI Agent Studio interface and configuration capabilities.

- These are new FAI-specific permission groups introduced with AI Agent Studio
- They control which product pillar's AI agents the user can configure in AI Agent Studio
- All follow the pattern: `ORA_DR_FAI_GENERATIVE_AI_AGENT_{PILLAR}_ADMINISTRATOR_DUTY`

---

## Security Model by Product Pillar

```
┌──────────────────────────────────────────────────────┐
│          AI AGENT STUDIO ACCESS MATRIX                   │
├───────────┬─────────────────┬───────────────────────┤
│ PRODUCT   │ ROLES & PRIV    │ ROLES & PERM GROUPS       │
├───────────┼─────────────────┼───────────────────────┤
│ HCM       │ HCM_MGMT_DUTY  │ FAI_HCM_ADMIN_DUTY        │
│ FIN       │ FIN_AGENT (x2) │ FAI_FIN_ADMIN_DUTY        │
│ SCM       │ (none extra)   │ FAI_SCM_ADMIN_DUTY        │
│ PRC       │ (none extra)   │ FAI_PRC_ADMIN_DUTY        │
│ CX/CRM    │ (none extra)   │ FAI_CX_ADMIN_DUTY         │
│ PSC       │ (none extra)   │ FAI_PSC_ADMIN_DUTY        │
│ GRC       │ (none extra)   │ FAI_GRC_ADMIN_DUTY        │
│ PRJ       │ (none extra)   │ FAI_PRJ_ADMIN_DUTY        │
│ ALL       │ MANAGE_ALL     │ All 8 FAI Admin Duties    │
└───────────┴─────────────────┴───────────────────────┘
```

---

## Why Two Separate Tabs?

Oracle uses two different security mechanisms for AI Agent Studio:

1. **Roles and Privileges Tab** controls the **functional security** — i.e., what data the user can interact with in the underlying Oracle Fusion module (HCM, FIN, etc.)

2. **Roles and Permission Groups Tab** controls the **AI Agent Studio platform access** — i.e., whether the user can open, create, and configure AI agents in the AI Agent Studio interface

Both must be configured for full access to work correctly.

---

## Security Best Practices

### 1. Principle of Least Privilege
- Only assign the specific product pillar duty roles the user needs
- Don't assign the "All Products" super admin role unless absolutely necessary
- Review and audit role assignments periodically

### 2. Custom Role Strategy
- Always create **custom job roles** rather than modifying predefined Oracle roles
- Follow a clear naming convention: e.g., `CUSTOM_AI_AGENT_{PRODUCT}_ADMIN`
- Document why each role was created and who it's assigned to

### 3. Segregation of Duties
- Separate AI Agent configuration access from AI Agent end-user access
- AI Agent builders should have Studio access
- End users of published agents typically don't need Studio access

### 4. Regular Audits
- Periodically review who has AI Agent Studio access
- Remove access for users who no longer need it
- Use Oracle Access Certifications where available

### 5. Test in Non-Production First
- Always test security configurations in a non-production (TEST/UAT) environment first
- Validate the user can see and configure agents before applying to production

---

## Related Oracle Security Documentation

- [Oracle Fusion Cloud Security Guide](https://docs.oracle.com/en/cloud/saas/)
- [Security Console Documentation](https://docs.oracle.com/en/cloud/saas/)
- [AI Agent Studio Access Requirements](https://docs.oracle.com/en/cloud/saas/fusion-ai/26b/aiaas/access-ai-agent-studio.html)
