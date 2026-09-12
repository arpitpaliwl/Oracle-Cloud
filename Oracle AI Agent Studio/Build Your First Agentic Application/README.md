# Oracle Fusion Agentic App — Invoice Aging Assistant

An end-to-end example of building an Oracle Fusion Agentic App that helps Accounts Payable users understand invoice aging, identify outstanding exposure, take contextual actions, and execute controlled business transactions.

## What I Built

The application is an **Invoice Aging Assistant** for Oracle Fusion Payables.

Instead of requiring a user to navigate through multiple ERP screens, the Agentic App brings together:

- Invoice aging analysis
- Executive-level visualization
- Invoice-level details
- Contextual business actions
- Invoice validation
- Payment creation
- Communications
- Ask Oracle

The goal was to demonstrate how an Agentic App can move from:

**Understand → Decide → Act → Communicate**

while keeping Oracle Fusion as the system of record.

---

# 1. The Business Problem

Accounts Payable users often need to answer questions such as:

- How much is currently unpaid?
- Which invoices are becoming overdue?
- Which aging bucket has the highest exposure?
- Which invoices require attention?
- Can I validate an invoice?
- Can I create a payment?
- What controls must be satisfied before payment?

Traditionally, answering these questions can require navigating across multiple screens and processes.

The idea behind this application was simple:

> What if the ERP could understand the user's intent, assemble the relevant information, highlight what matters, and provide the appropriate next action?

That became the **Invoice Aging Assistant**.

---

# 2. Agentic App vs AI Agent

One of the first design decisions was to separate the responsibilities of the AI Agent from the Agentic App.

### AI Agent

The AI Agent provides:

- Reasoning
- Data retrieval
- Business logic
- Tool invocation
- Decision making
- Transaction execution

### Agentic App

The Agentic App provides the user experience:

- Information Displays
- Actionable Insights
- Communications
- Ask Oracle
- Contextual actions

A simplified view:

```text
                  Agentic App
                       |
        +--------------+--------------+
        |              |              |
   Information     Actions       Communications
        |              |              |
        +--------------+--------------+
                       |
                  AI Agent
                       |
        +--------------+--------------+
        |              |              |
       Data          Logic          Actions
        |              |              |
                    Oracle Fusion
                 System of Record
