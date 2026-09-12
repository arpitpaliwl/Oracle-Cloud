# 04 — Code

---

# Build Invoice Aging

## Purpose

Calculate invoice aging deterministically. The Code node retrieves
invoice data and assigns unpaid installments to aging buckets.

## Code

``` javascript
var response = $context.$nodes.RETRIEVE_INVOICES.$output;
var currentDate = $context.$system.$currentDate;

function toDate(dateString) {
    if (!dateString) return null;

    var parts = String(dateString).substring(0, 10).split("-");

    if (parts.length !== 3) return null;

    return new Date(Date.UTC(
        Number(parts[0]),
        Number(parts[1]) - 1,
        Number(parts[2])
    ));
}

var today = toDate(currentDate);

var buckets = {
    "0-30 Days": { invoiceCount: 0, totalUnpaidAmount: 0 },
    "31-60 Days": { invoiceCount: 0, totalUnpaidAmount: 0 },
    "61-90 Days": { invoiceCount: 0, totalUnpaidAmount: 0 },
    "91-120 Days": { invoiceCount: 0, totalUnpaidAmount: 0 },
    "121+ Days": { invoiceCount: 0, totalUnpaidAmount: 0 }
};

if (!today || !response || !Array.isArray(response.items)) {
    return {
        summary: {
            totalUnpaidAmount: 0,
            totalOverdueInstallments: 0,
            highestExposureBucket: null,
            highestExposureAmount: 0
        },
        buckets: buckets
    };
}

response.items.forEach(function(invoice) {

    if (!invoice.invoiceInstallments ||
        !Array.isArray(invoice.invoiceInstallments.items)) {
        return;
    }

    invoice.invoiceInstallments.items.forEach(function(installment) {

        var unpaidAmount = Number(installment.UnpaidAmount);

        if (isNaN(unpaidAmount) || unpaidAmount <= 0) return;

        var dueDate = toDate(installment.DueDate);

        if (!dueDate) return;

        var agingDays = Math.floor(
            (today.getTime() - dueDate.getTime()) / 86400000
        );

        if (agingDays < 0) return;

        var bucket;

        if (agingDays <= 30) {
            bucket = "0-30 Days";
        } else if (agingDays <= 60) {
            bucket = "31-60 Days";
        } else if (agingDays <= 90) {
            bucket = "61-90 Days";
        } else if (agingDays <= 120) {
            bucket = "91-120 Days";
        } else {
            bucket = "121+ Days";
        }

        buckets[bucket].invoiceCount++;
        buckets[bucket].totalUnpaidAmount += unpaidAmount;
    });
});

var totalUnpaidAmount = 0;
var totalOverdueInstallments = 0;
var highestExposureBucket = null;
var highestExposureAmount = 0;

Object.keys(buckets).forEach(function(bucketName) {

    var bucket = buckets[bucketName];

    totalUnpaidAmount += bucket.totalUnpaidAmount;
    totalOverdueInstallments += bucket.invoiceCount;

    if (bucket.totalUnpaidAmount > highestExposureAmount) {
        highestExposureAmount = bucket.totalUnpaidAmount;
        highestExposureBucket = bucketName;
    }
});

return {
    summary: {
        totalUnpaidAmount: totalUnpaidAmount,
        totalOverdueInstallments: totalOverdueInstallments,
        highestExposureBucket: highestExposureBucket,
        highestExposureAmount: highestExposureAmount
    },
    buckets: buckets
};
```

## Design Decision

Aging is a business calculation, so it is performed deterministically in
code rather than by an LLM.

The LLM receives the result and is responsible for presentation.

---

# Check Search Intent

## Purpose

Validate the intent produced by the LLM before branching the query
workflow.

## Code

``` javascript
var searchContext = $context.$nodes.ANALYSE_INTENT.$output;

try {
  var cleaned = typeof searchContext === "string"
    ? searchContext.trim()
        .replace(/^```json\s*/i, "")
        .replace(/^```\s*/i, "")
        .replace(/\s*```$/, "")
    : searchContext;

  var parsed = typeof cleaned === "string"
    ? JSON.parse(cleaned)
    : cleaned;

  var validIntents = [
    "INVOICE_ANALYSIS",
    "INVOICE_ACTION",
    "OUTOFCONTEXT"
  ];

  if (parsed && validIntents.includes(parsed.intent)) {
    return parsed.intent;
  }
} catch (e) {
  console.error("Parsing error:", e);
}

return "OUTOFCONTEXT";
```

## Design Principle

The LLM proposes an intent.

The Code node validates it.

The workflow then branches using a known set of values.

---

# Extract Action

## Purpose

Parse the Agentic App action payload and identify the requested command
and business context.

## Code

``` javascript
var raw = $context.$app.$OraAction;

if (raw == null) {
    return {
        command: "UNKNOWN",
        AgingBucket: null,
        InvoiceId: null,
        InvoiceNumber: null
    };
}

var parsed = null;

if (typeof raw === "object") {
    parsed = raw;
} else if (typeof raw === "string") {
    try {
        parsed = JSON.parse(raw);
    } catch (e1) {
        try {
            var unescaped = raw.replace(/\"/g, '"');
            parsed = JSON.parse(unescaped);
        } catch (e2) {
            parsed = {};
        }
    }
}

if (!parsed || typeof parsed !== "object") {
    return {
        command: "UNKNOWN",
        AgingBucket: null,
        InvoiceId: null,
        InvoiceNumber: null
    };
}

if (parsed.command === "ValidateInvoice") {
    return {
        command: "ValidateInvoice",
        AgingBucket: null,
        InvoiceId: parsed.InvoiceId || null,
        InvoiceNumber: parsed.InvoiceNumber || null
    };
}

if (parsed.command === "CreatePayment") {
    return {
        command: "CreatePayment",
        AgingBucket: null,
        InvoiceId: parsed.InvoiceId || null,
        InvoiceNumber: parsed.InvoiceNumber || null
    };
}

if (parsed.AgingBucket) {
    return {
        command: "ViewDetails",
        AgingBucket: parsed.AgingBucket,
        InvoiceId: null,
        InvoiceNumber: null
    };
}

if (parsed.InvoiceId || parsed.InvoiceNumber) {
    return {
        command: "CreatePayment",
        AgingBucket: null,
        InvoiceId: parsed.InvoiceId || null,
        InvoiceNumber: parsed.InvoiceNumber || null
    };
}

return {
    command: "UNKNOWN",
    AgingBucket: null,
    InvoiceId: null,
    InvoiceNumber: null
};
```

## Design Note

Explicit commands are checked before falling back to invoice
identifiers.

This prevents a `ValidateInvoice` action from being incorrectly
interpreted as `CreatePayment`.

---

# Build Selected Bucket Invoices

## Purpose

Build the invoice detail dataset for the aging bucket selected by the
user.

## Code

``` javascript
var bucket =
    $context.$nodes.EXTRACT_ACTION.$output.result.AgingBucket;

var response =
    $context.$nodes.RETRIEVE_AGING_INVOICE.$output;

var currentDate =
    $context.$system.$currentDate;

function toDate(dateString) {
    if (!dateString) return null;

    var parts = String(dateString).substring(0, 10).split("-");

    if (parts.length !== 3) return null;

    return new Date(Date.UTC(
        Number(parts[0]),
        Number(parts[1]) - 1,
        Number(parts[2])
    ));
}

var today = toDate(currentDate);
var result = [];

if (!bucket || !today || !response || !Array.isArray(response.items)) {
    return result;
}

response.items.forEach(function(invoice) {

    if (!invoice.invoiceInstallments ||
        !Array.isArray(invoice.invoiceInstallments.items)) {
        return;
    }

    invoice.invoiceInstallments.items.forEach(function(installment) {

        var unpaidAmount = Number(installment.UnpaidAmount);

        if (isNaN(unpaidAmount) || unpaidAmount <= 0) return;

        var dueDate = toDate(installment.DueDate);

        if (!dueDate) return;

        var agingDays = Math.floor(
            (today.getTime() - dueDate.getTime()) / 86400000
        );

        if (agingDays < 0) return;

        var agingBucket;

        if (agingDays <= 30) {
            agingBucket = "0-30 Days";
        } else if (agingDays <= 60) {
            agingBucket = "31-60 Days";
        } else if (agingDays <= 90) {
            agingBucket = "61-90 Days";
        } else if (agingDays <= 120) {
            agingBucket = "91-120 Days";
        } else {
            agingBucket = "121+ Days";
        }

        if (agingBucket !== bucket) return;

        result.push({
            "Invoice ID": invoice.InvoiceId,
            "Invoice Number": invoice.InvoiceNumber,
            "Supplier": invoice.Supplier,
            "Supplier Site": invoice.SupplierSite,
            "Business Unit": invoice.BusinessUnit,
            "Invoice Currency": invoice.InvoiceCurrency,
            "Payment Method": installment.PaymentMethod,
            "Paid Status": invoice.PaidStatus,
            "Validation Status": invoice.ValidationStatus,
            "Invoice Date": invoice.InvoiceDate,
            "Installment Number": installment.InstallmentNumber,
            "Due Date": installment.DueDate,
            "Unpaid Amount": unpaidAmount,
            "Aging Days": agingDays
        });
    });
});

result.sort(function(a, b) {
    return b["Unpaid Amount"] - a["Unpaid Amount"];
});

return result;
```

## Design Note

The same aging rule is used for the executive summary and the drill-down
so the two views remain consistent.

---

# Check Payment Eligibility

## Purpose

Determine whether an invoice satisfies the business conditions required
for payment creation.

## Code

``` javascript
var response =
    $context.$nodes.RETRIEVE_INVOICE_FOR_PAYMENT.$output;

var invoice =
    response &&
    Array.isArray(response.items) &&
    response.items.length > 0
        ? response.items[0]
        : null;

if (!invoice) {
    return {
        eligible: false,
        reason: "Invoice not found"
    };
}

var validationStatus =
    String(invoice.ValidationStatus || "").trim();

var approvalStatus =
    String(invoice.ApprovalStatus || "").trim();

var paidStatus =
    String(invoice.PaidStatus || "").trim();

var validApprovalStatuses = [
    "Not required",
    "Approved",
    "Workflow Approved",
    "Manually Approved"
];

var validationEligible =
    validationStatus === "Validated";

var approvalEligible =
    validApprovalStatuses.indexOf(approvalStatus) !== -1;

var unpaidEligible =
    paidStatus === "Unpaid";

return {
    eligible:
        validationEligible &&
        approvalEligible &&
        unpaidEligible,

    ValidationStatus: validationStatus,
    ApprovalStatus: approvalStatus,
    PaidStatus: paidStatus,

    validationEligible: validationEligible,
    approvalEligible: approvalEligible,
    unpaidEligible: unpaidEligible,

    reason:
        !validationEligible
            ? "Invoice is not validated."
            : !approvalEligible
                ? "Invoice approval is not complete."
                : !unpaidEligible
                    ? "Invoice is already paid."
                    : null
};
```

## Business Rule

Payment creation requires:

``` text
ValidationStatus = Validated

AND

ApprovalStatus ∈
- Not required
- Approved
- Workflow Approved
- Manually Approved

AND

PaidStatus = Unpaid
```

The LLM does not decide payment eligibility.

---

# Build Payment Data

## Purpose

Construct the payment payload after eligibility has been confirmed.

## Code

``` javascript
var response =
    $context.$nodes.RETRIEVE_INVOICE_FOR_PAYMENT.$output;

var invoice =
    response &&
    Array.isArray(response.items) &&
    response.items.length > 0
        ? response.items[0]
        : null;

if (!invoice) {
    return { error: "Invoice not found" };
}

var installments =
    invoice.invoiceInstallments &&
    Array.isArray(invoice.invoiceInstallments.items)
        ? invoice.invoiceInstallments.items
        : [];

var installment = null;

for (var i = 0; i < installments.length; i++) {
    var item = installments[i];

    if (
        Number(item.UnpaidAmount) > 0 &&
        item.HoldFlag !== true
    ) {
        installment = item;
        break;
    }
}

if (!installment) {
    return {
        error: "No unpaid installment available for payment"
    };
}

var paymentYear =
    String($context.$system.$currentDate).substring(0, 4);

var paymentNumber =
    Number(paymentYear + String(invoice.InvoiceId));

return {
    PaymentNumber: paymentNumber,
    PaymentDate: $context.$system.$currentDate,
    PaymentDescription: "Manual_Payment",
    PaymentType: "Manual",
    PaymentCurrency:
        invoice.PaymentCurrency ||
        invoice.InvoiceCurrency,
    BusinessUnit: invoice.BusinessUnit,
    Payee: invoice.Supplier,
    PayeeSite: invoice.SupplierSite,
    DisbursementBankAccountName: "BofA-2869",
    PaymentMethod:
        installment.PaymentMethod ||
        invoice.PaymentMethod,
    PaymentProcessProfile:
        installment.PaymentMethodCode === "CHECK"
            ? "Standard Check - All Currency"
            : "SWIFT MT100",
    PaymentDocument: "",
    relatedInvoices: [
        {
            InvoiceNumber: invoice.InvoiceNumber,
            InstallmentNumber:
                Number(installment.InstallmentNumber),
            AmountPaidPaymentCurrency:
                Number(installment.UnpaidAmount)
        }
    ]
};
```

## Design Note

The payment payload is constructed only after the eligibility gate
succeeds.

---

# Build Validation Payload

## Purpose

Build the payload required by the Oracle Fusion invoice validation
action.

## Code

``` javascript
return {
  InvoiceNumber: invoice.InvoiceNumber,
  BusinessUnit: invoice.BusinessUnit,
  Supplier: invoice.Supplier,
  ProcessAction: "Validate"
};
```

## Payload

``` json
{
  "InvoiceNumber": "...",
  "BusinessUnit": "...",
  "Supplier": "...",
  "ProcessAction": "Validate"
}
```

The payload is derived from the retrieved invoice rather than being
invented by the LLM.
