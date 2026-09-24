Create exactly THREE independent, deterministic policy functions
from the provided Expense Reimbursement Policy document.

Do not create a single combined function.

The three functions must be:

1. evaluateExpenseEligibility
2. calculateReimbursementAmount
3. determineApprovalRequirement

These functions will be consumed by an Oracle Fusion AI Agent
Studio Workflow Agent. Therefore, preserve the exact function
names, input parameter names, output parameter names, and data
types specified below.

============================================================
FUNCTION 1: evaluateExpenseEligibility
============================================================

Purpose:
Determine whether an employee expense is eligible for
reimbursement based strictly on the supplied policy document.

Inputs:
- expenseCategory: string
- expenseAmount: number
- expenseDate: string (YYYY-MM-DD)
- submissionDate: string (YYYY-MM-DD)
- receiptProvided: boolean
- businessPurpose: string
- expenseStatus: string

Outputs:
- eligible: boolean
- decision: string
- reasonCode: string

Allowed decision values:
- ELIGIBLE
- NOT_ELIGIBLE

Allowed reasonCode values:
- WITHIN_POLICY
- MISSING_RECEIPT
- NON_REIMBURSABLE_CATEGORY
- INVALID_SUBMISSION
- OUT_OF_DATE
- INVALID_AMOUNT

Implementation requirements:

1. Evaluate the expense against all applicable eligibility
   rules defined in the supplied policy document.

2. Return eligible = true and decision = ELIGIBLE only when
   all applicable eligibility requirements are satisfied.

3. Return eligible = false and decision = NOT_ELIGIBLE when
   any eligibility requirement is violated.

4. The reasonCode must identify the actual policy rule
   responsible for the decision.

5. Apply the following reason-code mapping:

   - WITHIN_POLICY:
     The expense satisfies all applicable eligibility rules.

   - MISSING_RECEIPT:
     A receipt is required under the policy but was not
     provided.

   - NON_REIMBURSABLE_CATEGORY:
     The expense category is explicitly excluded from
     reimbursement by the policy.

   - INVALID_SUBMISSION:
     The expense status, business purpose, or another
     submission requirement violates the policy.

   - OUT_OF_DATE:
     The expense was submitted outside the permitted
     submission timeframe.

   - INVALID_AMOUNT:
     The expense amount violates the policy's amount
     validity requirements.

6. Return exactly one reasonCode from the allowed list.
   Do not return multiple codes, concatenated codes, or
   custom codes.

7. When multiple eligibility rules fail, use a consistent
   precedence based on the policy document. If the document
   does not specify precedence, establish a deterministic
   precedence among the listed reason codes without changing
   the underlying eligibility rules.

8. Do not return WITHIN_POLICY if any eligibility condition
   has failed.

9. Do not invent additional eligibility requirements.

10. Handle null values according to the supplied policy.
    Do not treat Boolean false as null or as a missing value.

11. Use YYYY-MM-DD date handling.

12. An expense submitted exactly 60 calendar days after
    the expense date is eligible under the timeframe rule.
    An expense submitted after 61 calendar days is not
    eligible under that rule.

13. An expense amount of exactly 25 USD does not require
    a receipt. An amount greater than 25 USD requires a
    receipt.

============================================================
FUNCTION 2: calculateReimbursementAmount
============================================================

Purpose:
Calculate the reimbursable amount for an expense using
the provided eligibility result and applicable reimbursement
limits defined in the supplied policy document.

Inputs:
- expenseCategory: string
- expenseAmount: number
- eligible: boolean

Outputs:
- reimbursableAmount: number
- amountDecision: string
- limitApplied: number

Allowed amountDecision values:
- FULL_REIMBURSEMENT
- POLICY_LIMIT_APPLIED
- NOT_ELIGIBLE

Implementation requirements:

1. Use the provided eligible input as the eligibility
   decision. Do not independently evaluate eligibility.

2. If eligible = false, return:
   reimbursableAmount = 0
   amountDecision = NOT_ELIGIBLE
   limitApplied = 0

3. If eligible = true, calculate the reimbursable amount
   using the applicable category-specific reimbursement
   limit defined in the policy.

4. Return FULL_REIMBURSEMENT when the full submitted
   expense amount is reimbursable.

5. Return POLICY_LIMIT_APPLIED when the policy limit
   reduces the reimbursable amount below the submitted
   expense amount.

6. Return NOT_ELIGIBLE only when the provided eligible
   input is false.

7. Apply these category limits:

   - MEAL: maximum reimbursement of 75 USD.
   - HOTEL: maximum reimbursement of 250 USD.
   - GROUND_TRANSPORTATION: maximum reimbursement of
     100 USD.

8. For categories with a defined cap:

   - If the expense amount is less than or equal to the
     applicable cap, reimburse the full expense amount.
   - If the expense amount exceeds the applicable cap,
     reimburse only the cap.

9. Exactly 75 USD for a meal, 250 USD for a hotel, and
   100 USD for ground transportation are fully reimbursable
   under the respective cap.

10. Define limitApplied consistently:

    - Return 0 when no reduction is applied.
    - When a policy cap reduces the reimbursement, return
      the difference between the submitted expense amount
      and the reimbursable amount.

11. Do not confuse the policy cap with the amount deducted.
    For example, if a meal expense is 100 USD and the
    maximum reimbursement is 75 USD:
    reimbursableAmount = 75
    amountDecision = POLICY_LIMIT_APPLIED
    limitApplied = 25

12. For eligible expense categories without a specified
    reimbursement cap, apply the policy's stated
    reimbursement rule. Do not invent a cap.

13. Use deterministic calculations only. Do not use
    probabilistic reasoning or LLM-based judgment.

14. Handle null values according to the supplied policy.

15. Do not calculate approval requirements in this function.

============================================================
FUNCTION 3: determineApprovalRequirement
============================================================

Purpose:
Determine whether approval is required and identify the
correct approval level using the reimbursable amount and
the approval hierarchy defined in the supplied policy.

Inputs:
- reimbursableAmount: number
- expenseCategory: string

Outputs:
- approvalRequired: boolean
- approvalLevel: string

Allowed approvalLevel values:
- MANAGER
- FINANCE_MANAGER
- FINANCE_DIRECTOR

Implementation requirements:

1. Determine the approval level using the
   reimbursableAmount input, not the original submitted
   expense amount.

2. Apply the approval thresholds and hierarchy exactly
   as defined in the supplied policy document.

3. The approval level must reflect the applicable
   threshold. Do not automatically return FINANCE_DIRECTOR.

4. Apply the following approval hierarchy:

   - Reimbursable amount below 500 USD:
     approvalLevel = MANAGER

   - Reimbursable amount greater than or equal to
     500 USD and below 2,000 USD:
     approvalLevel = FINANCE_MANAGER

   - Reimbursable amount greater than or equal to
     2,000 USD:
     approvalLevel = FINANCE_DIRECTOR

5. Exactly 500 USD requires FINANCE_MANAGER approval.

6. Exactly 2,000 USD requires FINANCE_DIRECTOR approval.

7. Return the approval level corresponding to the
   applicable threshold. Do not return the highest
   approval level by default.

8. Set approvalRequired according to the approval
   requirement defined in the policy.

9. Do not infer an approval level from expenseCategory
   unless the supplied policy explicitly defines
   category-specific approval rules.

10. Do not recalculate the reimbursement amount in
    this function.

11. Do not use the original expense amount or the
    expense eligibility decision to override the
    provided reimbursableAmount.

12. Handle null values according to the supplied policy.
    Do not invent an approval level for undefined or
    invalid inputs.

============================================================
GLOBAL IMPLEMENTATION RULES
============================================================

1. Implement only business rules contained in the
   supplied Expense Reimbursement Policy document and
   explicitly specified in this prompt.

2. Do not invent additional policy rules, thresholds,
   categories, exceptions, or approval levels.

3. Generate exactly three independent policy functions.

4. Each function must be independently callable.

5. Use deterministic executable logic only.

6. Do not use probabilistic reasoning or LLM-based
   decision-making inside the policy functions.

7. Treat Boolean false as a valid value.

8. Handle null values according to the policy document.
   Do not silently substitute defaults.

9. Preserve the exact function names and parameter names.

10. Return only the specified output parameters for
    each function. Do not add extra output fields.

11. Do not return explanatory prose, summaries, or
    additional fields outside the defined output schema.

12. Ensure the functions can be chained in a Workflow Agent:

    evaluateExpenseEligibility
             |
             v
    calculateReimbursementAmount
             |
             v
    determineApprovalRequirement

13. The eligibility function's eligible output must map
    to the reimbursement function's eligible input.

14. The reimbursement function's reimbursableAmount
    output must map to the approval function's
    reimbursableAmount input.

15. Review the generated function logic and test cases
    for correct boundary handling, policy compliance,
    and consistent outputs.

16. Generate test cases covering valid inputs, invalid
    inputs, null handling, exact policy thresholds,
    and values immediately above and below each
    applicable threshold.

17. Do not publish functions until their logic and
    test results have been reviewed.

Generate the three policy functions independently,
with exact schemas and deterministic behavior suitable
for use in an Oracle Fusion AI Agent Studio Workflow Agent.
