# Solution Overview

## Objective

Demonstrate a policy-driven approach in Oracle Fusion AI Agent Studio: convert an illustrative written expense policy into deterministic functions, then orchestrate them in a Workflow Agent.

## Separation of responsibilities

| Function | Responsibility | Key output |
|---|---|---|
| `evaluateExpenseEligibility` | Eligibility only | `eligible`, `decision`, `reasonCode` |
| `calculateReimbursementAmount` | Reimbursement cap only; trusts supplied `eligible` | `reimbursableAmount`, `amountDecision`, `limitApplied` |
| `determineApprovalRequirement` | Approval tier based on reimbursable amount | `approvalRequired`, `approvalLevel` |

Keeping these responsibilities separate supports independent testing, clearer rule ownership, and easier maintenance when policy changes.

## Workflow sequence

1. Map the expense attributes into `evaluateExpenseEligibility`.
2. Pass its `eligible` output and the required expense attributes to `calculateReimbursementAmount`.
3. Pass `reimbursableAmount` to `determineApprovalRequirement`.
4. Present or route the resulting decision according to the workflow design and configured controls.

## Control points

- Treat policy text as the authoritative business-rule source for this demo.
- Do not allow the LLM to improvise eligibility, caps, or approval tiers inside the deterministic functions.
- Review generated code/function definitions before publishing.
- Test exact thresholds, values immediately above/below thresholds, nulls, and multiple simultaneous failures.
- Preserve traceability from each function rule back to the source policy.
- Approval tier determination is not the same as executing an approval or payment.

## Known specification consideration

The source policy specifies valid reason codes but does not define precedence when several eligibility conditions fail at once. The generation prompt explicitly asks for deterministic precedence. Record and test the selected precedence rather than assuming it is policy-mandated.
