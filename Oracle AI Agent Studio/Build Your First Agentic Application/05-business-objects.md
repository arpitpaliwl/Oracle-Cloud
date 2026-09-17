# Business Objects — Payables Invoice Aging Assistant

This document captures the Business Objects used by the **Payables Invoice Aging Assistant** built in Oracle AI Agent Studio.

The definitions below are taken directly from the Business Object exports used in the build.

---

## 1. Payables Invoice Aging Inquiry

**Business Object Name:** `Payables Invoice Aging Inquiry`  
**Object Code:** `ORA_FIN_PAYABLES_PAYABLESINVOICEAGINGINQUIRY`  
**Supported Object ID:** `300000340258768`  
**Description:** `Retrieves unpaid invoice details`  
**Family:** `FIN`  
**Product:** `PAYABLES`  
**Object Source:** `ADF_BC`  
**REST Resource:** `/fscmRestApi/resources/11.13.18.05/invoices`  
**Seeded:** `false`  
**Mandatory:** `false`

This Business Object provides the invoice data used by the Invoice Aging Assistant.

### Tools

This Business Object exposes two tools:

1. `getall_invoices`
2. `get_invoices_byid`

---

### 1.1 `getall_invoices`

**Description:** `Fetch a paged invoice collection by paid status`  
**Operation:** `GET`  
**Operation ID:** `getall_invoices`  
**Resource Type:** `ADF_BC_FIXED_QUERY`  
**Authentication:** Native authentication  
**Header:**

```text
REST-Framework-Version: 9
```

**Parameter:**

| Parameter | Data Type | Description | Location |
|---|---|---|---|
| `BusinessUnit` | string | Business Unit | query |

**Default Value:** empty string

### REST Resource Path

```text
/fscmRestApi/resources/11.13.18.05/invoices?q=PaidStatus!='Paid' and BusinessUnit='{BusinessUnit}' and ValidationStatus!='Canceled'&limit=500&onlyData=true&totalResults=true&expand=invoiceInstallments&fields=InvoiceId,InvoiceNumber,InvoiceCurrency,PaymentCurrency,BusinessUnit,Supplier,SupplierSite,PaymentMethod,PaidStatus,ValidationStatus,InvoiceDate;invoiceInstallments:InstallmentNumber,UnpaidAmount,DueDate
```

### Data returned

Invoice-level fields:

```text
InvoiceId
InvoiceNumber
InvoiceCurrency
PaymentCurrency
BusinessUnit
Supplier
SupplierSite
PaymentMethod
PaidStatus
ValidationStatus
InvoiceDate
```

Expanded child collection:

```text
invoiceInstallments
```

Installment fields:

```text
InstallmentNumber
UnpaidAmount
DueDate
```

### Query behavior

The configured query:

- excludes invoices where `PaidStatus = 'Paid'`
- filters by the supplied `BusinessUnit`
- excludes invoices where `ValidationStatus = 'Canceled'`
- retrieves up to 500 records
- requests `onlyData=true`
- requests total result information
- expands `invoiceInstallments`

---

### 1.2 `get_invoices_byid`

**Description:** `Fetch a invoice collection by invoiceid`  
**Operation:** `GET`  
**Operation ID:** `get_invoices_byid`  
**Resource Type:** `ADF_BC_FIXED_QUERY`  
**Authentication:** Native authentication  
**Header:**

```text
REST-Framework-Version: 9
```

**Parameter:**

| Parameter | Data Type | Description | Location |
|---|---|---|---|
| `InvoiceID` | string | Invoice ID | query |

**Default Value:** empty string

### REST Resource Path

```text
/fscmRestApi/resources/11.13.18.05/invoices?expand=invoiceInstallments&limit=500&offset=0&onlyData=true&totalResults=true&q=BusinessUnit='US1 Business Unit' and PaidStatus!='Paid' and ValidationStatus!='Canceled' and InvoiceId = '{InvoiceID}'
```

### Invoice fields returned

The sample Business Object response exposes the following invoice-level fields:

```text
InvoiceId
InvoiceNumber
InvoiceCurrency
PaymentCurrency
InvoiceAmount
InvoiceDate
BusinessUnit
Supplier
SupplierNumber
ProcurementBU
SupplierSite
RequesterId
Requester
InvoiceGroup
ConversionRateType
ConversionDate
ConversionRate
AccountingDate
Description
DeliveryChannelCode
DeliveryChannel
PayAloneFlag
InvoiceSourceCode
InvoiceSource
InvoiceType
PayGroup
InvoiceReceivedDate
PaymentReasonCode
PaymentReason
PaymentReasonComments
RemittanceMessageOne
RemittanceMessageTwo
RemittanceMessageThree
PaymentTerms
TermsDate
GoodsReceivedDate
PaymentMethodCode
PaymentMethod
SupplierTaxRegistrationNumber
FirstPartyTaxRegistrationId
FirstPartyTaxRegistrationNumber
LegalEntity
LegalEntityIdentifier
LiabilityDistribution
DocumentCategory
DocumentSequence
VoucherNumber
ValidationStatus
ApprovalStatus
PaidStatus
AccountingStatus
ApplyAfterDate
CanceledFlag
AmountPaid
BaseAmount
PurchaseOrderNumber
Party
PartySite
ControlAmount
DocumentFiscalClassificationCodePath
TaxationCountry
RoutingAttribute1
RoutingAttribute2
RoutingAttribute3
RoutingAttribute4
RoutingAttribute5
AccountCodingStatus
BudgetDate
FundsStatus
CanceledDate
CanceledBy
UniqueRemittanceIdentifier
UniqueRemittanceIdentifierCheckDigit
CreationDate
CreatedBy
LastUpdatedBy
LastUpdateDate
LastUpdateLogin
BankAccount
SupplierIBAN
ExternalBankAccountId
BankChargeBearer
SettlementPriority
ReferenceKeyOne
ReferenceKeyTwo
ReferenceKeyThree
ReferenceKeyFour
ReferenceKeyFive
ProductTable
ImageDocumentNumber
DigitalPaymentAccount
StreamDetailId
Stream
DocumentName
DueDate
ExceptionCode
Exception
InvoiceStatusCode
InvoiceStatus
StreamDefinitionCode
```

### `invoiceInstallments`

The expanded `invoiceInstallments` child collection exposes:

```text
InstallmentNumber
UnpaidAmount
FirstDiscountAmount
FirstDiscountDate
DueDate
GrossAmount
HoldReason
PaymentPriority
SecondDiscountAmount
SecondDiscountDate
ThirdDiscountAmount
ThirdDiscountDate
NetAmountOne
NetAmountTwo
NetAmountThree
HoldFlag
HeldBy
HoldType
PaymentMethod
PaymentMethodCode
HoldDate
BankAccount
ExternalBankAccountId
CreatedBy
CreationDate
LastUpdateDate
LastUpdatedBy
LastUpdateLogin
RemitToAddressName
RemitToSupplier
RemittanceMessageOne
RemittanceMessageTwo
RemittanceMessageThree
DigitalPaymentAccount
```

---

## 2. Validate Invoice

**Business Object Name:** `Validate Invoice`  
**Object Code:** `ORA_FIN_PAYABLES_VALIDATEINVOICE`  
**Supported Object ID:** `300000340611710`  
**Description:** `Validate single payables invoice`  
**Family:** `FIN`  
**Product:** `PAYABLES`  
**Object Source:** `ADF_BC`  
**REST Resource:** `/fscmRestApi/resources/11.13.18.05/invoices`  
**Seeded:** `false`  
**Mandatory:** `false`

### Tool

```text
doall_validateInvoice_invoices
```

**Description:** `validateInvoice`  
**Operation:** `POST`  
**Operation ID:** `doall_validateInvoice_invoices`  
**Resource Type:** `ADF_BC_FIXED_QUERY`  
**Authentication:** Native authentication

### REST Resource Path

```text
/fscmRestApi/resources/11.13.18.05/invoices/action/validateInvoice
```

### Parameter

| Parameter | Data Type | Description | Location |
|---|---|---|---|
| `Payload` | string | payload for validating the invoice | body |

**Body Template:**

```text
{Payload}
```

### Header

```text
Content-Type: application/vnd.oracle.adf.action+json
```

### Sample payload

```json
{
  "InvoiceNumber": "REST_Invoice",
  "BusinessUnit": "Vision Operations",
  "Supplier": "Advanced Network Devices",
  "ProcessAction": "Validate"
}
```

This Business Object is used by the Agentic App action that validates a selected Payables invoice before the payment flow proceeds.

---

## 3. Payables Payments Creation

**Business Object Name:** `Payables Payments Creation`  
**Object Code:** `ORA_FIN_PAYABLES_PAYABLESPAYMENTSCREATION`  
**Supported Object ID:** `300000339952591`  
**Description:** `Payment Payments creation`  
**Family:** `FIN`  
**Product:** `PAYABLES`  
**Object Source:** `ADF_BC`  
**REST Resource:** `/fscmRestApi/resources/11.13.18.05/payablesPayments`  
**Seeded:** `false`  
**Mandatory:** `false`

### Tool

```text
create_payablesPayments
```

**Description:** `Create Payables Payments`  
**Operation:** `POST`  
**Operation ID:** `create_payablesPayments`  
**Resource Type:** `ADF_BC_FIXED_QUERY`  
**Authentication:** Native authentication

### REST Resource Path

```text
/fscmRestApi/resources/11.13.18.05/payablesPayments
```

### Parameter

| Parameter | Data Type | Description | Location |
|---|---|---|---|
| `Payload` | string | Payable Payment Payload | body |

**Body Template:**

```text
{Payload}
```

### Payment payload structure

The Business Object sample payload contains:

```text
CheckId
PaymentNumber
PaymentReference
PaymentAmount
PaymentDate
PaymentStatus
AccountingStatus
PaymentType
PaymentCurrency
BusinessUnit
Payee
SupplierNumber
PayeeSite
PaymentMethod
PaymentProcessProfile
DisbursementBankAccountName
relatedInvoices
```

### `relatedInvoices`

Each related invoice can contain:

```text
InvoicePaymentId
InvoiceId
InvoiceBusinessUnit
InvoiceNumber
InstallmentNumber
AmountPaidPaymentCurrency
DiscountTaken
InvoiceCurrency
```

### Sample payload

```json
{
  "CheckId": 300100174803470,
  "PaymentNumber": 1013,
  "PaymentReference": "PMT-20240615-001",
  "PaymentAmount": 1250.75,
  "PaymentDate": "2024-06-15",
  "PaymentStatus": "Completed",
  "AccountingStatus": "Posted",
  "PaymentType": "Manual",
  "PaymentCurrency": "USD",
  "BusinessUnit": "Vision Operations",
  "Payee": "Acme Supplies Inc.",
  "SupplierNumber": "SUPP-100234",
  "PayeeSite": "Main Warehouse",
  "PaymentMethod": "Wire Transfer",
  "PaymentProcessProfile": "Standard Wire",
  "DisbursementBankAccountName": "BofA-204",
  "relatedInvoices": [
    {
      "InvoicePaymentId": 300100174803471,
      "InvoiceId": 355913,
      "InvoiceBusinessUnit": "Vision Operations",
      "InvoiceNumber": "INV-20240601-1001",
      "InstallmentNumber": 1,
      "AmountPaidPaymentCurrency": 1250.75,
      "DiscountTaken": 50,
      "InvoiceCurrency": "USD"
    }
  ]
}
```

---

# 4. How the Three Business Objects Work Together

The three Business Objects support different parts of the Agentic App:

```text
                    Payables Invoice
                    Aging Inquiry
                         │
                         │ Retrieve invoice
                         │ and installment data
                         ▼
                 Invoice Aging Assistant
                         │
             ┌───────────┴───────────┐
             │                       │
       Validate Invoice       Create Payment
             │                       │
             ▼                       ▼
       Validate Invoice      Payables Payments
          Business Object         Creation
```

### Functional responsibility

| Business Object | Primary responsibility | Operation |
|---|---|---|
| `Payables Invoice Aging Inquiry` | Retrieve unpaid invoice and installment information | GET |
| `Validate Invoice` | Validate a Payables invoice | POST |
| `Payables Payments Creation` | Create a Payables payment | POST |

---

# 5. Agentic App Flow

The Business Objects are used as part of the broader Workflow Agent and Agentic App experience:

```text
User
  │
  ▼
Agentic App
  │
  ▼
Workflow Agent
  │
  ├── Invoice Aging Inquiry
  │       └── Retrieve invoice + installment data
  │
  ├── Validate Invoice
  │       └── Validate selected invoice
  │
  └── Payables Payments Creation
          └── Create eligible payment
```

The Business Objects provide the connection to Fusion business data and transactions, while the Workflow Agent controls orchestration and the Agentic App provides the user interaction layer.

---

# 6. Important Design Principle

The Business Objects should not be treated as a replacement for workflow/business-rule logic.

For this implementation:

```text
Business Object
      ↓
Fusion Data / Transaction
      ↓
Workflow Logic
      ↓
Deterministic Business Rules
      ↓
Agentic App Action
```

The LLM can understand the user's request and determine which workflow behavior is appropriate, while deterministic logic should control calculations, validation conditions, eligibility checks, and transaction payload construction.

This separation helps keep financial processing predictable and governed.

---

## 7. Fusion REST API Version

All three Business Objects in these exports use the Fusion REST API version:

```text
11.13.18.05
```

The configured resources are:

```text
Invoices
/fscmRestApi/resources/11.13.18.05/invoices

Invoice Validation
/fscmRestApi/resources/11.13.18.05/invoices/action/validateInvoice

Payables Payments
/fscmRestApi/resources/11.13.18.05/payablesPayments
```

> When reproducing this example in another Fusion environment, verify the available Business Object configuration, REST resource version, attributes, security roles, and transaction permissions in that environment.
