# 02 — Prompts

---

# InitDisplay Prompt

## Purpose

Generate the initial executive view of invoice aging.

## Prompt

``` text
Display an executive view of the invoice aging data from:

{{$context.$nodes.BUILD_INVOICE_AGING.$output.result}}

The supplied data is authoritative.

The business calculations have already been performed by the Code node.

Do not recalculate, modify, infer, or fabricate any business values.

Use only these supported widgets:
- Chart
- Multi Record

Create the display in this exact order.

SECTION 1 — AGING DISTRIBUTION

Create a Chart widget.

Use a bar chart.

Title:
Invoice Aging Distribution

Use the exact aging bucket names from:

buckets

Use the corresponding totalUnpaidAmount values as the chart values.

Include every aging bucket returned by the workflow, including zero-value buckets.

Use the values exactly as provided.

Do not calculate or modify chart values.

SECTION 2 — AGING DETAILS

Create a Multi Record widget.

Title:
Review Invoice Aging

Display these columns:

- Aging Bucket
- Invoice Count
- Total Unpaid Amount

Create one row for every bucket in:
{{$context.$nodes.BUILD_INVOICE_AGING.$output.result.buckets}}

The number of table rows must exactly equal the number of returned aging buckets.

Do not filter, summarize, merge, or omit rows.

Add a "View Details" action to every row.

For each row invoke:

ora.Invoke(
  "ViewDetails",
  "{"command":"ViewDetails","AgingBucket":"<current row Aging Bucket>"}"
)

The AgingBucket value must come exactly from the current row.

Format Total Unpaid Amount as currency.

Do not display individual invoices in this initial view.

Do not create forms.
Do not create dropdowns.
Do not create Submit buttons.
Do not create Card widgets.
Do not use the Record widget.
Do not use pattern.list.simple.

Do not add recommendations.
Do not add explanatory paragraphs.
Do not add introductory text.
Do not add concluding text.

The final display should contain only:

1. Invoice Aging Distribution chart
2. Review Invoice Aging table

If no aging data exists, display only:

No invoice aging data found.
```

## Prompt Design Notes

The important constraints are:

-   Data is authoritative.
-   Business calculations happen before presentation.
-   Widget types are explicitly restricted.
-   Every aging bucket must be rendered.
-   Row-level action context comes from the current row.
-   No unsupported UI is allowed.

---

# InitDisplay System Prompt

## Purpose

Restrict generated information displays to configured Agentic Apps
widgets.

## Prompt

``` text
Generate only valid Oracle Agentic Apps information display widgets supported by this agent.

Use only the widgets selected and configured for this agent.

Do not generate unsupported component types or pattern IDs.

Do not generate forms, dropdowns, or submit buttons.

Follow the requested widget type and structure exactly.
```

## Why

The display prompt defines the desired experience, while the system
prompt provides a higher-level guardrail against unsupported UI
generation.

---

# Invoice Details Prompt

## Purpose

Display invoices selected from an aging bucket and expose the
appropriate invoice-level action.

## Prompt

``` text
Display the invoices from:
{{$context.$nodes.BUILD_SELECTED_BUCKET_INVOICES.$output.result}}

Render the data using the Multi Record widget.

Title:
Invoices in {{$context.$nodes.EXTRACT_ACTION.$output.result.AgingBucket}} Bucket

Display these columns:
- Invoice Number
- Supplier
- Supplier Site
- Business Unit
- Invoice Date
- Due Date
- Unpaid Amount
- Aging Days
- Paid Status
- Validation Status

For each row:

If Validation Status exactly equals "Validated":
  action text = Create Payment

  invoke:
  ora.Invoke(
    "CreatePayment",
    "{"command":"CreatePayment","InvoiceId":"<current row Invoice ID>","InvoiceNumber":"<current row Invoice Number>"}"
  )

Otherwise:
  action text = Validate Invoice

  invoke:
  ora.Invoke(
    "ValidateInvoice",
    "{"command":"ValidateInvoice","InvoiceId":"<current row Invoice ID>","InvoiceNumber":"<current row Invoice Number>"}"
  )

The InvoiceId and InvoiceNumber must come exactly from the current row data.

Format monetary values using the Invoice Currency.

Format dates as:
MM-DD-YYYY

Display Paid Status and Validation Status as badge/pill-style values.

Do not display:
- Invoice ID as a column
- Payment Currency
- API metadata
- Technical fields

Do not create charts.
Do not create additional widgets.
Do not add recommendations.
Do not add explanatory text.
Do not add introductory text.
Do not add concluding text.

The Multi Record table must be the only content displayed by this node.

If the data is empty, display only:
No invoices found.
```

## Key Design Decision

The action is conditional on current business state. The application
does not expose both `Validate Invoice` and `Create Payment`
simultaneously.

---

# Invoice Table Prompt

## Purpose

Display invoice search results using the Multi Record widget.

## Prompt

``` text
Display the invoices from:
{{$context.$nodes.CREATE_INVOICE_TABLE_DATA.$output.result}}

Render the data using the Multi Record widget.

Display the relevant business columns supplied by the workflow.

Create one row for every invoice returned by the workflow.

The number of table rows must exactly equal the number of returned records.

Do not filter, summarize, merge, or omit records.

If a field is missing, display a blank value or — rather than omitting the row.

Do not independently decide that records are irrelevant.

Do not truncate the dataset.

Do not create charts.

Do not create additional widgets.

Do not add recommendations.

Do not add explanatory text before or after the table.

If no invoices are returned, display only:

No invoices found.
```

## Why

The display layer should not independently reduce the dataset. Upstream
retrieval and data preparation determine the relevant records; the
display layer renders them faithfully.

---

# Payment Eligibility Prompt

## Purpose

Explain why payment creation cannot proceed when deterministic
eligibility checks fail.

## Prompt

``` text
Explain to the user why the payment cannot be created.

Use this payment eligibility result:
{{$context.$nodes.CHECK_PAYMENT_ELIGIBILITY.$output.result}}

If the invoice is not validated, explain that the invoice must be validated before payment can be created.

If the approval status is not one of:
- Not required
- Approved
- Workflow Approved
- Manually Approved

explain that the invoice approval is not complete and show the current approval status.

If the invoice is not unpaid, explain that payment cannot be created because the invoice is not currently unpaid.

Do not create a payment.
Do not fabricate any information.
Keep the response concise.
```

The LLM explains the result of a deterministic decision; it does not
decide eligibility.

---

# Payment Result Prompt

## Purpose

Display the business outcome of payment creation without exposing
unnecessary API metadata.

## Prompt

``` text
Display the result of the payment creation.

Payment creation response:
{{$context.$nodes.CREATE_PAYMENT.$output.result}}

If the response contains a successfully created payment, confirm that the payment was created successfully.

Display only the following information when available:
- Payment Number
- Payment Amount
- Payment Currency
- Payment Date
- Payment Method
- Payment Process Profile
- Payment Document
- Payee
- Business Unit
- Related Invoice Number
- Installment Number
- Amount Paid

Use values exactly as returned by the payment creation response.

For relatedInvoices, use values from relatedInvoices array.

Do not display:
- CheckId
- PaymentId
- PaymentFileReference
- PaymentProcessRequest
- API links
- Technical metadata
- CreatedBy
- LastUpdatedBy
- CreationDate
- LastUpdateDate
- changeIndicator
- Any other technical fields

Do not fabricate or infer missing values.

If response does not indicate successful payment creation, clearly state payment could not be created and display relevant error info.

Do not attempt to create another payment.

Keep concise.
```

The user needs the business outcome, not the raw API response.

---

# Query Intent Prompt

## Purpose

The query workflow first determines what kind of invoice-related request
the user is making.

Supported intents:

``` text
INVOICE_ANALYSIS
INVOICE_ACTION
OUTOFCONTEXT
```

## Flow

``` text
User Question
      |
      v
Analyse Intent
      |
      +---- INVOICE_ANALYSIS
      |
      +---- INVOICE_ACTION
      |
      +---- OUTOFCONTEXT
```

## Design Principle

The LLM identifies intent.

A Code node validates the returned intent before the workflow branches.

This separates understanding from execution.

---

# Communication Prompt

## Purpose

Generate a clean HTML email body for the `Invoice Aging Summary`
communication.

## Prompt Pattern

``` text
Generate the Invoice Aging Summary as an HTML email body.

Return HTML markup only.

Do not return Markdown.

Do not include ```html.

Do not include ```.

Do not add an explanation.

The output itself is the email body.

Use a clean enterprise-style HTML table.

Use:

<table style="border-collapse:collapse;width:100%;font-family:Arial,sans-serif;">
<thead>
...
</thead>
<tbody>
...
</tbody>
</table>

Use clear table headers, cell padding, borders, and appropriate alignment.

Keep the summary outside the table with clear spacing.

Use values from the supplied workflow context.

Do not fabricate values.
```

## Lesson

Communication prompts need an explicit output-format contract. Otherwise
an LLM may return Markdown or fenced code instead of email-ready HTML.
