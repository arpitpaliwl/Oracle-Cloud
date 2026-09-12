# 01 — Architecture

---


## Overview

The Invoice Aging Assistant uses a workflow-driven Agentic App
architecture.

The primary domain agent is:

`Payables Invoice Aging`

The same agent supports multiple application behaviors based on
`OraMessageHint`.

## High-Level Architecture

``` text
User
 |
 v
Agentic App
 |
 +--> Summary
 +--> InitDisplay
 +--> InitActions
 +--> InitCommunications
 +--> Query
 +--> InvokeAction
 |
 v
Payables Invoice Aging Agent
 |
 +--> Oracle Fusion Business Objects
 +--> Code Nodes
 +--> Oracle Fusion REST APIs
 |
 v
Oracle Fusion
System of Record
```

## Message Hint Routing

``` text
OraMessageHint
      |
      v
"What kind of interaction is happening?"
      |
      v
Workflow Switch
      |
      +---- Summary
      +---- InitDisplay
      +---- InitActions
      +---- InitCommunications
      +---- Query
      +---- InvokeAction
```

The message hint is the signal. The workflow is the router. The
resulting branch is the behavior.

## Context

The implementation separates interaction type from business context.

``` text
Message Hint
=
What job are we doing?

Context
=
What are we doing it with?
```

Important runtime context includes:

``` text
$context.$system.$inputMessage
$context.$system.$chatHistory
$context.$app.$OraAction
```

## Payment Control Gate

``` text
Create Payment
      |
      v
Retrieve Invoice
      |
      v
Check Eligibility
      |
      +---- Validation Status
      +---- Approval Status
      +---- Paid Status
      |
      +---- Eligible ------> Build Payment Data
      |                           |
      |                           v
      |                     Create Payment
      |
      +---- Not Eligible --> Explain Reason
```

The transaction is controlled by deterministic workflow conditions.

> **AI provides the intelligence. The workflow provides the control.**

---


## Goal

Move the AP user from an executive understanding of invoice exposure to
a controlled business action without navigating across multiple ERP
screens.

## Journey

``` text
Open App
   |
   v
Understand Aging
   |
   v
Identify Exposure
   |
   v
Select Aging Bucket
   |
   v
Review Invoices
   |
   +----------------------+
   |                      |
   v                      v
Validate Invoice      Create Payment
   |                      |
   |                Eligibility Gate
   |                      |
   |                +-----+-----+
   |                |           |
   |              Allowed     Blocked
   |                |           |
   |                v           v
   |            Payment      Explain
   |            Creation     Reason
   |                |
   +----------------+
            |
            v
       Business Outcome
            |
            v
       Communication
```

## Executive View

The application opens with:

1.  Invoice Aging Distribution
2.  Review Invoice Aging

The user starts with exposure, not raw invoice records.

## Drill Down

The user selects `View Details`.

Example action context:

``` json
{
  "command": "ViewDetails",
  "AgingBucket": "31-60 Days"
}
```

## Invoice-Level Decision

``` text
Validated
   |
   +--> Create Payment

Not Validated
   |
   +--> Validate Invoice
```

## Payment Decision

Payment creation is protected by deterministic eligibility checks:

``` text
Validated?
Approved?
Unpaid?
```

Only when the required conditions are satisfied does the workflow
proceed.

## Design Outcome

**Understand → Decide → Act → Communicate**
