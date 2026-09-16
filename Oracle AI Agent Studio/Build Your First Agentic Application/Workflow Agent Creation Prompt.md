Create a new workflow agent named **Payables Invoice Aging Demo** with code `PAYABLES_INVOICE_AGING_DEMO`, family `FIN`, product `PAYABLES`, architecture `data_pipeline`, REST trigger, no human approval, and no wait flag.

**Purpose:** Analyze invoice aging and allow users to review, validate, and create payments for eligible invoices.

Build the workflow with a **Context Switcher** immediately after START using:
`{{$context.$app.$OraMessageHint}}`

Create these six branches:

* **Query** → Analyse Intent
* **InitDisplay** → Retrieve Invoices
* **InitActions** → Plan Create Payment
* **InvokeAction** → Extract Action
* **Summary** → Fetch Summary Invoice
* **InitCommunications** → Display Communications

All branches must end at a shared END node.

Which business object and function to use (use these exact names everywhere)
To look up many invoices at once (used in InitDisplay, Summary, and ViewDetails): business object <>. Every time you call this, pass BusinessUnit = "<>" — don't skip this, it's required every time, not just once.
To validate an invoice: business object <>
To create a payment: business object <>.

### InitDisplay

Retrieve invoices for `US1 Business Unit`, calculate overdue aging using the current date, and group unpaid installments into:

* 0-30 Days
* 31-60 Days
* 61-90 Days
* 91-120 Days
* 121+ Days

Display the five buckets with invoice count and total unpaid amount. Include a **View Details** action for each bucket.
[BO] Retrieve Invoices → [Code] Build Invoice Aging (bucket unpaid installments by due date into 0-30 / 31-60 / 61-90 / 91-120 / 121+ Days, always show all 5) → [LLM] Display Invoice Aging (table, one row per bucket, "View Details" action) → END

### Summary

Retrieve invoices, calculate the same aging buckets plus total overdue installments, total unpaid amount, highest unpaid bucket, highest invoice-count bucket, and 121+ Days totals.

[BO] Fetch Summary Invoice → [Code] Build Invoice Aging Summary (same buckets + totals + highest bucket) → [LLM] Display Aging Summary (short bullet summary) → END

Display a short executive summary.

### InitActions

Surface one action:
**Review Invoice Aging** → opens the invoice review/details experience. Do not create a payment here.

[LLM] Plan Create Payment (one action: "Review Invoice Aging", doesn't create a payment) → END

### Query

Use an LLM to classify the user's request as:

* `INVOICE_ANALYSIS`
* `INVOICE_ACTION`
* `OUTOFCONTEXT`

For analysis requests, extract relevant invoice filters such as invoice number, supplier, status, dates, amount, currency, PO number, and top N.

If `INVOICE_ANALYSIS`, retrieve invoices, apply the extracted filters, and display the results in a table.

Otherwise, return a concise out-of-context response.

### InvokeAction

Parse `$context.$app.$OraAction` and identify:

* `ViewDetails`
* `CreatePayment`
* `ValidateInvoice`

[Code] Extract Action (parse $OraAction, which may be an object, a JSON string, or an escaped JSON string — handle all 3; identify ViewDetails / CreatePayment / ValidateInvoice, else UNKNOWN) → [Switch] Action Condition Check → 3 branches


Important — read this carefully: $OraAction won't always arrive in a clean, ready-to-use format. Sometimes it's already a usable object, sometimes it's a text string that looks like JSON, and sometimes that text string has extra quote marks around it that need to be cleaned up first. Handle all three cases so the parsing doesn't fail. If none of the three known commands match after parsing, return UNKNOWN instead of crashing or leaving it blank.

**ViewDetails:** Retrieve invoices (same business object/function/BusinessUnit), filter by the selected aging bucket, and display invoice details. Show Validate Invoice for unvalidated invoices and Create Payment for validated invoices.
[BO] Retrieve Aging Invoice → [Code] Build Selected Bucket Invoices (filter to the clicked bucket) → [LLM] Display Invoice Details (per row: show "Validate Invoice" if unvalidated, "Create Payment" if validated) → END

**ValidateInvoice:** Retrieve the invoice by ID, build the validation payload, call the invoice validation API, and display the result.
[BO] Retrieve Invoice for Validation → [Code] Build Validation Payload → [BO] Validate Invoice → [LLM] Display Validation Result → END

**CreatePayment:** Retrieve the invoice and verify that it is validated and approved. If eligible, build the payment payload and call the payment creation API. Otherwise, display the reason payment cannot be created.
[BO] Retrieve Invoice For Payment → [Code] Check Payment Eligibility (eligible only if ValidationStatus = "Validated)[If] eligible?
True → [Code] Build Payment Data (use your real bank account and payment method values, not placeholders) → [BO] Create Payment  → [LLM] Display Payment Data → END
False → [LLM] Display Eligibility (explain which condition failed) → END

### InitCommunications

Prepare the invoice communication experience only. Do not send emails or modify invoices/payments.

The actual email template and recipient are maintained in the application configuration, not in this workflow.

Keep it concise — only build what's listed above, no extra nodes or widgets.

Legend: [BO] = Business Object Function node. [Code] = Code node. [LLM] = LLM node. [Switch] = Switch node. [If] = Condition node. Rule: Build every node in the list below as its own node, in order, each connected to the next. Every branch must end on an [LLM] node — never on [Code] or [BO].
Make sure all nodes are connected at the end and synced

Note: the actual "Invoice Aging Summary" email — its HTML template, its table of aging buckets, and its recipient — lives in the app config (PAYABLES_INVOICE_AGING.json → templates[]) as an app-defined communication of type "email", not inside this workflow. This branch only decides what to surface as available to send; drafting and sending the templated content is handled by the Communications framework at the app level. If your builder generates workflows and app configs separately, ask for the template as a second step.
