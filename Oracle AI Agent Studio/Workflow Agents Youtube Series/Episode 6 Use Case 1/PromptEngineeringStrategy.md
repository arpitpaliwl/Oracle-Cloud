# Prompt Engineering Strategy

A major portion of this workflow agent relies on carefully engineered LLM prompts to ensure deterministic ERP transaction processing. 
Instead of using generic conversational AI prompts, the workflow uses structured enterprise prompts optimized for:

* JSON extraction
* Oracle Fusion payload generation
* PO matching
* Supplier validation
* Human approval interpretation
* ERP-safe response formatting

---

# Key Prompt Design Principles

## 1. Strict JSON Enforcement

All payload-generation prompts explicitly enforce valid JSON output.

Example:

* No markdown fences
* No conversational text
* No explanations
* JSON-only response

This minimizes downstream parsing failures and improves automation reliability.

---

# Invoice Extraction Prompt

The invoice parsing node extracts:

* Invoice Number
* Invoice Date
* Due Date
* Payment Terms
* Supplier Information
* PO Number
* Invoice Lines
* Quantities
* Unit Prices
* Total Amount

Additional validations included:

* Currency normalization
* Date standardization
* Line amount reconciliation
* PO number detection logic
* Multi-line extraction support

The prompt also includes mathematical sanity validation to compare line totals vs invoice header totals.

---

# Supplier Site Validation Prompt

The workflow uses AI reasoning to evaluate supplier site and procurement BU combinations retrieved from Oracle Fusion.

The prompt dynamically:

* Detects whether single or multiple supplier sites exist
* Generates clean review output
* Structures supplier site recommendations
* Prepares data for Human-in-the-Loop approval

This reduces manual AP validation effort significantly.

---

# Human Approval Interpretation Prompt

After human review, another LLM node interprets free-text user feedback.

Example user inputs:

* "Use US1"
* "First option"
* "Use Vision BU"

The LLM maps these responses back into:

* Supplier Site
* Procurement BU

This creates a conversational approval experience instead of rigid dropdown-based selection.

---

# PO Invoice Payload Generation Prompt

For PO-based invoices, the LLM:

* Combines extracted invoice data
* Combines Oracle Fusion PO header data
* Combines PO line data
* Performs PO line matching

Matching logic includes:

* Description matching
* Unit price matching
* PO line number mapping

The final result is a fully structured Oracle Fusion AP Invoice payload.

---

# Non-PO Payload Generation Prompt

For non-PO invoices, the AI:

* Uses validated supplier site information
* Maps procurement business units
* Builds invoice lines dynamically
* Constructs Oracle Fusion invoice payload JSON

---

# Confirmation Message Prompt

The final LLM nodes generate highly readable operational summaries for AP users.

These prompts:

* Convert raw API responses into business-friendly summaries
* Format line details cleanly
* Highlight invoice numbers and amounts
* Improve operational readability

---

# Why Prompt Engineering Matters Here

This workflow agent demonstrates that enterprise AI automation is not only about:

* APIs
* Integrations
* Workflow orchestration

It also depends heavily on:

* deterministic prompting
* structured output enforcement
* validation logic
* AI reasoning constraints
* ERP-safe payload generation

The prompts effectively act as:

* AI transformation layers
* ERP translators
* validation engines
* payload builders

inside the workflow architecture.

---

# Enterprise AI Design Pattern Used

This workflow agent follows a common enterprise AI orchestration pattern:

Input → Extraction → Validation → Reasoning → Human Approval → Payload Generation → ERP Transaction Creation

Where LLM nodes are used as:

* Intelligent parsers
* Validation engines
* Decision support systems
* Structured payload generators

instead of simple chatbots.
