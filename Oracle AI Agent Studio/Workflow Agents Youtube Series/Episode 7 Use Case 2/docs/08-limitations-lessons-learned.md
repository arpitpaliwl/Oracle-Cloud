# Limitations and Lessons Learned

## Overview

This document captures the current limitations, architectural trade-offs, implementation assumptions, and lessons learned during the development of the Project Resource Replacement AI Agent.

Understanding these considerations is critical before deploying the AI Agents into production environments.

---

# Current Architecture Review

The AI Agent successfully demonstrates:

* Oracle AI Agent Studio orchestration
* HCM and Projects integration
* BI Publisher integration
* LLM-assisted workflow execution
* Dynamic payload generation
* Parallel project processing

However, several design considerations should be reviewed before enterprise-wide deployment.

---

# Limitation 1 – Duplicate Worker Names

## Current Design

Worker lookup uses:

```http id="frj1nh"
DisplayName LIKE '%John%Smith%'
```

---

## Risk

Multiple workers may share similar names.

Example:

```text id="4jlwm7"
John Smith
John A Smith
John Smith Jr
```

Potential outcome:

Wrong worker selected.

---

## Impact

High

---

## Recommendation

Use:

```text id="v43h4j"
Person Number
Employee Number
Email Address
```

instead of display name matching.

---

# Limitation 2 – No Resource Availability Validation

## Current Design

Incoming resource is assigned automatically.

No validation is performed for:

* Resource utilization
* Capacity
* Existing workload
* Availability

---

## Risk

Resource may already be fully allocated.

---

## Recommendation

Future enhancement:

```text id="z4cd5z"
Resource Capacity Validation
```

before assignment creation.

---

# Limitation 3 – No Approval Workflow

## Current Design

Execution begins immediately.

---

## Risk

Incorrect requests can result in staffing changes.

---

## Recommendation

Introduce:

```text id="4v5z8m"
Manager Approval

PMO Approval

Project Owner Approval
```

before execution.

---

# Limitation 4 – No Rollback Strategy

## Current Design

Execution order:

```text id="l5pq53"
End-Date Assignment

Create Assignment
```

---

## Risk

If create fails:

```text id="qkxvsz"
Outgoing Resource Removed

Incoming Resource Not Created
```

Project may temporarily lose staffing.

---

## Recommendation

Implement:

```text id="3nqiv6"
Error Logic for each Node

Rollback Workflow

```

---

# Limitation 5 – LLM-Based CSV Parsing

## Current Design

LLM converts:

```text id="5br3fo"
XML
→ Base64
→ CSV
→ JSON
```

---

## Benefits

Flexible implementation.

---

## Risks

* Potential parsing errors
* Increased execution time

---

# Limitation 6 – Large Volume Processing

## Current Design

Parallel project processing.

---

## Benefits

Improved performance.

---

## Risks

Large numbers of projects may generate:

* API throttling
* Timeout conditions

---

## Recommendation

Introduce:

```text id="1rfj3g"
Batch Processing

```

---

# Limitation 7 – Project Discovery Dependency

## Current Design

Project discovery relies on:

```text id="ndx9e8"
PROJECT_RESOURCE_REPORT
```

---

## Risk

If report becomes unavailable:

```text id="v1t3h1"
Project Discovery Fails
```

---

## Recommendation

Create alternate discovery options.

---

# Lessons Learned

## Lesson 1

Natural Language Interfaces Improve Adoption

Users prefer:

```text id="mlj3vh"
Replace John Smith with Sarah Johnson
```

instead of navigating multiple application screens.

---

## Lesson 2

AI Works Best as an Orchestrator

The accelerator demonstrates AI generating:

* Structured outputs
* Payloads
* Transformations

rather than replacing business systems.

---

## Lesson 3

Oracle Fusion APIs Enable Strong Automation

Using supported APIs ensures:

* Maintainability
* Supportability
* Upgrade compatibility

---

## Lesson 4

Role Inheritance Reduces Errors

Automatically inheriting the outgoing role simplifies staffing transitions.

---

## Lesson 5

BI Publisher Can Be an Effective Discovery Layer

Reports can expose business relationships not easily accessible through a single API call.

---

## Lesson 6

Parallel Processing Significantly Improves Scalability

Processing projects simultaneously reduces execution time.

---

## Lesson 7

Structured Prompting Improves Reliability

Explicit JSON-only outputs significantly reduce variability.

---

# Recommended Production Enhancements

| Enhancement                  | Priority |
| ---------------------------- | -------- |
| Approval Workflow            | High     |
| Rollback Capability          | High     |
| Resource Capacity Validation | High     |
| Dry Run Mode                 | High     |
| Batch Processing             | Medium   |
| Audit Dashboard              | Medium   |

---

# Key Takeaways

The AI Agent demonstrates a strong reference architecture for Oracle AI Agent Studio. While suitable proof-of-concept, production deployments should strengthen validation, approvals, rollback handling, auditability, and resource governance.
