# 03 — Workflows and Actions

---

# InitDisplay Workflow

## Flow

``` text
InitDisplay
    |
    v
Retrieve Invoices
    |
    v
Build Invoice Aging
    |
    v
Display Aging
    |
    +--> Chart
    |
    +--> Multi Record
```

## Build Invoice Aging

The Code node calculates:

-   Aging days
-   Aging bucket
-   Invoice count
-   Total unpaid amount
-   Total overdue installments
-   Highest exposure bucket
-   Highest exposure amount

## Display

The display layer renders:

1.  `Invoice Aging Distribution`
2.  `Review Invoice Aging`

The display layer does not recalculate values.

---

# Query Workflow

## Purpose

Handle natural-language questions and determine whether the user is
asking for invoice analysis, an invoice action, or something outside the
application context.

## Flow

``` text
Query
 |
 v
Analyse Intent
 |
 v
Check Search Intent
 |
 v
If
 |
 +---- true
 |      |
 |      v
 |   Fetch Invoices
 |      |
 |      v
 |   Create Invoice Table Data
 |      |
 |      v
 |   Display Table
 |
 +---- false
        |
        v
   No Context Exists
```

## Intent Values

``` text
INVOICE_ANALYSIS
INVOICE_ACTION
OUTOFCONTEXT
```

The LLM interprets the request, the Code node validates the intent, and
the workflow controls the branch.

---

# Communications Workflow

## Purpose

Provide an `Invoice Aging Summary` communication experience.

## Communication

The demo uses:

``` text
Invoice Aging Summary
```

It can contain:

-   Aging bucket
-   Invoice count
-   Outstanding amount
-   Executive summary

## Output Format

The email body is generated as HTML.

The prompt requires:

``` text
HTML markup only
No Markdown
No fenced code blocks
No explanation
```

## Simplified Scope

The demonstration keeps communications intentionally simple and focuses
on the communication experience.

A fuller implementation can separate:

``` text
InitCommunications
      ↓
FillParameters
      ↓
SendCommunication
```

---

# Validate Invoice Action

## Purpose

Validate an invoice when its current validation status does not allow
payment creation.

## Flow

``` text
ValidateInvoice
      |
      v
Retrieve Invoice For Validation
      |
      v
Build Validation Payload
      |
      v
Validate Invoice REST / Business Action
      |
      v
Display Validation Result
```

## Request

``` text
POST /fscmRestApi/resources/11.13.18.05/invoices/action/validateInvoice
```

Required content type:

``` text
application/vnd.oracle.adf.action+json
```

Example payload:

``` json
{
  "InvoiceNumber": "REST_Invoice",
  "BusinessUnit": "Vision Operations",
  "Supplier": "Advanced Network Devices",
  "ProcessAction": "Validate"
}
```

## Success Response

``` json
{
  "result": "The current action Validate Invoice has completed successfully."
}
```

## Agentic App Action Chain

``` text
Start
  ↓
Send Agent Command
  ↓
Refresh Agents
```

The action uses the payload as the command and refreshes the agent after
the data-changing operation.

---

# Create Payment Action

## Purpose

Create a payment for an eligible invoice.

## Flow

``` text
CreatePayment
  |
  v
Retrieve Invoice For Payment
  |
  v
Check Payment Eligibility
  |
  +---- FALSE ---> Display Eligibility
  |
  +---- TRUE ----> Build Payment Data
                         |
                         v
                    Create Payment
                         |
                         v
                  Display Payment
```

## Eligibility Gate

Payment creation requires:

``` text
ValidationStatus = Validated
```

Approval status must be one of:

``` text
Not required
Approved
Workflow Approved
Manually Approved
```

And:

``` text
PaidStatus = Unpaid
```

## Payment API

``` text
/fscmRestApi/resources/11.13.18.05/payablesPayments
```

## Demo Rules

Payment number:

``` text
numeric year + InvoiceId
```

Example:

``` text
2026 + 1578378 = 20261578378
```

Other demo values:

``` text
PaymentDescription = Manual_Payment
PaymentType = Manual
PaymentDate = System Date
```

Payment process profile:

``` text
CHECK
  -> Standard Check - All Currency

Other
  -> SWIFT MT100
```

Adapt these values to the target Fusion environment before production
use.

---

# Refresh Agent

## Purpose

Refresh the Agentic App's agent data after a data-changing action.

## Action Chain

``` text
Start
  ↓
Send Agent Command
  ↓
Refresh Agents
```

## Why

Invoice validation and payment creation change the underlying Fusion
business state.

Refreshing the agent allows the application to reflect the updated
state.

## Pattern

``` text
Execute Business Action
        ↓
Refresh Agent State
        ↓
Present Updated Experience
```

---

# View Details Payload

``` json
{
  "command": "ViewDetails",
  "AgingBucket": "31-60 Days"
}
```

The `AgingBucket` value is taken from the selected Multi Record row.

---

# Validate Invoice Payload

``` json
{
  "command": "ValidateInvoice",
  "InvoiceId": "1578378",
  "InvoiceNumber": "INV-10001"
}
```

The identifiers must come from the current invoice row.

---

# Create Payment Payload

``` json
{
  "command": "CreatePayment",
  "InvoiceId": "1578378",
  "InvoiceNumber": "INV-10001"
}
```

The identifiers must come from the current invoice row.
