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

### InitDisplay

Retrieve invoices for `US1 Business Unit`, calculate overdue aging using the current date, and group unpaid installments into:

* 0-30 Days
* 31-60 Days
* 61-90 Days
* 91-120 Days
* 121+ Days

Display the five buckets with invoice count and total unpaid amount. Include a **View Details** action for each bucket.

### Summary

Retrieve invoices, calculate the same aging buckets plus total overdue installments, total unpaid amount, highest unpaid bucket, highest invoice-count bucket, and 121+ Days totals.

Display a short executive summary.

### InitActions

Surface one action:
**Review Invoice Aging** → opens the invoice review/details experience. Do not create a payment here.

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

**ViewDetails:** Retrieve invoices, filter by the selected aging bucket, and display invoice details. Show **Validate Invoice** for unvalidated invoices and **Create Payment** for validated invoices.

**ValidateInvoice:** Retrieve the invoice, build the validation payload, call the invoice validation API, and display the result.

**CreatePayment:** Retrieve the invoice and verify that it is validated and approved. If eligible, build the payment payload and call the payment creation API. Otherwise, display the reason payment cannot be created.

### InitCommunications

Prepare the invoice communication experience only. Do not send emails or modify invoices/payments.

The actual email template and recipient are maintained in the application configuration, not in this workflow.

Keep the workflow implementation concise and focus on the required **nodes, connections, conditions, and business logic**. Do not add unnecessary nodes, explanations, widgets, or functionality.
Note: the actual "Invoice Aging Summary" email — its HTML template, its table of aging buckets, and its recipient — lives in the app config (PAYABLES_INVOICE_AGING.json → templates[]) as an app-defined communication of type "email", not inside this workflow. This branch only decides what to surface as available to send; drafting and sending the templated content is handled by the Communications framework at the app level. If your builder generates workflows and app configs separately, ask for the template as a second step.
