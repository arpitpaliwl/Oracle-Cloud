# Oracle Fusion Agentic App --- Invoice Aging Assistant

An end-to-end example of building an Oracle Fusion Agentic App for
invoice aging, contextual invoice actions, payment controls, validation,
and communications.

## What I Built

The application is an **Invoice Aging Assistant** for Oracle Fusion
Payables.

It brings together: - Invoice aging analysis - Executive visualization -
Invoice-level details - Contextual actions - Invoice validation -
Payment creation - Communications - Ask Oracle

The overall pattern is:

**Understand → Decide → Act → Communicate**

Oracle Fusion remains the system of record.

## Agent vs Agentic App

### AI Agent

Reasoning, data retrieval, business logic, tool invocation, decision
support, and transaction execution.

### Agentic App

Information Displays, Actionable Insights, Communications, Ask Oracle,
and contextual actions.

The Agentic App is the user-facing interaction layer over enterprise
agents and Fusion business processes.

## Four Pillars

1.  **Information Displays** --- show the right information immediately.
2.  **Actionable Insights** --- move from information to the next
    business action.
3.  **Communications** --- distribute business information.
4.  **Ask Oracle** --- continue interacting with the underlying agent
    using natural language.

## Architecture

``` text
                         Agentic App
                              |
                       OraMessageHint
                              |
                       Workflow Switch
                              |
          +-------------------+-------------------+
          |                   |                   |
       Summary           InitDisplay            Query
          |                   |                   |
          |             Chart + Table       Intent Analysis
          |                                       |
          |                              +--------+--------+
          |                              |                 |
          |                         Analysis           Action
          |                              |                 |
          +------------------------------+-----------------+
                                         |
                                   InvokeAction
                                         |
                           +-------------+-------------+
                           |                           |
                    Validate Invoice              Create Payment
                           |                           |
                           +-------------+-------------+
                                         |
                                  Oracle Fusion
                                   System of Record
```

## Design Principles

### Deterministic business calculations

A Code node calculates invoice aging, unpaid exposure, and bucket
assignment. The LLM does not calculate financial values.

### Explicit UI contracts

Prompts define which widgets can be used, which fields must be
displayed, which rows must be rendered, and which actions are available.

### Explicit action context

Row-level actions pass business context such as `AgingBucket`,
`InvoiceId`, and `InvoiceNumber`.

### Workflow controls transactions

The LLM can understand intent, but deterministic workflow logic controls
whether a transaction can proceed.

### Business outcome over API metadata

The final experience exposes business-relevant payment information
rather than raw REST response metadata.

## Repository Structure

``` text
architecture/
prompts/
code/
actions/
workflows/
examples/
screenshots/
```

## Key Takeaway

An Agentic App is not simply:

> Put an AI chatbot on top of ERP.

A stronger pattern is:

``` text
Understand
    ↓
Present
    ↓
Decide
    ↓
Control
    ↓
Act
    ↓
Communicate
```

AI provides the intelligence.

The workflow provides the control.

Oracle Fusion remains the system of record.
