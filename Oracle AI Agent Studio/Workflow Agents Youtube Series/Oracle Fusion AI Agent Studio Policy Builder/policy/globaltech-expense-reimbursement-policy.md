# GlobalTech Expense Reimbursement Policy

## 1\. Purpose

This policy defines the rules used to determine:

1. Whether an employee expense is eligible for reimbursement.
2. The maximum amount that can be reimbursed.
3. The approval level required for the reimbursable amount.

These rules are illustrative business policies for this demonstration.

\---

# 2\. Expense Eligibility Rules

An expense is eligible for reimbursement only when all applicable
eligibility requirements are satisfied.

## 2.1 Expense Status

The expense must have a status of:

SUBMITTED

Any other status is not eligible for reimbursement.

## 2.2 Business Purpose

The expense must contain a valid business purpose.

If the business purpose is null, empty, or contains only whitespace,
the expense is not eligible.

## 2.3 Expense Amount

The expense amount must be greater than zero.

An expense with an amount of zero or less is not eligible.

## 2.4 Expense Submission Date

The expense date must not be more than 60 calendar days before
the submission date.

If the difference between submission date and expense date
is greater than 60 days, the expense is not eligible.

## 2.5 Non-Reimbursable Categories

The following expense categories are never reimbursable:

* PERSONAL
* ALCOHOL
* FINES
* PERSONAL\_ENTERTAINMENT

If the expense belongs to one of these categories,
the expense is not eligible.

## 2.6 Receipt Requirement

A receipt is required when the expense amount is greater than
25 USD.

If the expense amount is greater than 25 USD and receiptProvided
is false, the expense is not eligible.

An expense of exactly 25 USD does not require a receipt.

\---

# 3\. Reimbursement Amount Rules

These rules apply only when the expense has been determined
to be eligible.

## 3.1 Meals

The maximum reimbursable amount for a meal expense is:

75 USD

If the submitted meal expense is greater than 75 USD,
the reimbursable amount is limited to 75 USD.

## 3.2 Hotel

The maximum reimbursable amount for a hotel expense is:

250 USD

If the submitted hotel expense is greater than 250 USD,
the reimbursable amount is limited to 250 USD.

## 3.3 Ground Transportation

The maximum reimbursable amount for ground transportation is:

100 USD

If the submitted ground transportation expense is greater than
100 USD, the reimbursable amount is limited to 100 USD.

## 3.4 Other Business Expenses

For other eligible business expenses, the full submitted amount
is reimbursable.

\---

# 4\. Approval Rules

Approval is determined using the reimbursable amount.

## 4.1 Manager Approval

If reimbursable amount is less than 500 USD:

approvalRequired = true
approvalLevel = MANAGER

## 4.2 Finance Manager Approval

If reimbursable amount is greater than or equal to 500 USD
and less than 2,000 USD:

approvalRequired = true
approvalLevel = FINANCE\_MANAGER

## 4.3 Finance Director Approval

If reimbursable amount is greater than or equal to 2,000 USD:

approvalRequired = true
approvalLevel = FINANCE\_DIRECTOR

\---

# 5\. Eligibility Decision Codes

Use only the following reason codes:

WITHIN\_POLICY
MISSING\_RECEIPT
NON\_REIMBURSABLE\_CATEGORY
INVALID\_SUBMISSION
OUT\_OF\_DATE
INVALID\_AMOUNT

Decision values:

ELIGIBLE
NOT\_ELIGIBLE

\---

# 6\. Reimbursement Decision Codes

Use only:

FULL\_REIMBURSEMENT
POLICY\_LIMIT\_APPLIED
NOT\_ELIGIBLE

\---

# 7\. Approval Levels

Use only:

MANAGER
FINANCE\_MANAGER
FINANCE\_DIRECTOR

\---

# 8\. Function Requirements

The policy implementation must contain exactly three
independent functions:

1. evaluateExpenseEligibility
2. calculateReimbursementAmount
3. determineApprovalRequirement

Each function must have explicitly defined input and output
parameters.

The functions must implement only the rules contained
in this document.

Do not infer additional business rules.

Do not combine the three functions into one function.

\---

# 9\. Null and Invalid Input Handling

For the eligibility function:

* Null expenseCategory must result in NOT\_ELIGIBLE.
* Null expenseAmount must result in NOT\_ELIGIBLE.
* Null expenseDate must result in NOT\_ELIGIBLE.
* Null submissionDate must result in NOT\_ELIGIBLE.
* Null expenseStatus must result in NOT\_ELIGIBLE.

For businessPurpose, null, empty, or whitespace-only values
are invalid.

For receiptProvided, false must be treated as a valid Boolean
value and must not be interpreted as null.

\---

# 10\. Date Handling

Calculate the number of calendar days between expenseDate
and submissionDate.

An expense is within the submission period when the difference
is less than or equal to 60 days.

Exactly 60 days is eligible.

61 days is not eligible.

\---

# 11\. Boundary Conditions

The implementation must correctly handle:

* expenseAmount = 0
* expenseAmount < 0
* expenseAmount = 25
* expenseAmount > 25
* meal amount = 75
* meal amount > 75
* hotel amount = 250
* hotel amount > 250
* ground transportation amount = 100
* ground transportation amount > 100
* reimbursableAmount = 500
* reimbursableAmount = 2,000
* expense age = 60 days
* expense age = 61 days

Boundary values must follow the rules above exactly.

