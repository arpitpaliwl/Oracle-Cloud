# Oracle Fusion AP Invoice Processor – Architecture

## Solution Overview

This solution demonstrates an AI-powered Accounts Payable Invoice Processing Workflow Agent built using:

* Oracle AI Agent Studio
* Oracle Fusion Financials
* Oracle Fusion REST APIs
* Workflow Agents
* LLM-based reasoning
* Human-in-the-Loop approvals

The workflow agent processes invoice PDFs and creates AP invoices automatically inside Oracle Fusion.

---

# Core Architecture Pattern

The solution follows an enterprise AI orchestration pattern:

Invoice Input
→ AI Extraction
→ Validation
→ Decision Routing
→ Human Review
→ Payload Generation
→ Oracle Fusion Transaction Creation

---

# Workflow Architecture

## Stage 1 – Invoice Intake

### Node Type

Tool Node

### Component Used

Chat Attachment Reader

### Purpose

* Accept invoice PDF upload
* Extract raw invoice text
* Extract metadata for downstream AI processing

---

# Stage 2 – Invoice Parsing

### Node Type

LLM Node

### Purpose

AI extracts:

* Invoice Number
* Invoice Date
* Due Date
* Supplier Name
* Invoice Amount
* PO Number
* Invoice Lines

### Output

Structured JSON payload

---

# Stage 3 – Workflow Decision Routing

### Node Type

Condition Node

### Logic

Checks whether:

* PO Number exists
* PO Number is null

### Routing

#### PO Exists

→ PO Invoice Flow

#### PO Missing

→ Non-PO Invoice Flow

---

# PO Invoice Flow

## Step 1 – Fetch PO Details

### Node Type

Business Object Function

### Oracle API

purchaseOrders REST API

### Purpose

Retrieve:

* PO Header
* Supplier Site
* Procurement BU

---

## Step 2 – Fetch PO Lines

### Node Type

Business Object Function

### Purpose

Retrieve PO lines for matching

---

## Step 3 – PO Line Matching

### Node Type

LLM Node

### Matching Logic

AI compares:

* Description
* Unit Price
* Quantity

between:

* invoice lines
* PO lines

---

## Step 4 – Build PO Invoice Payload

### Node Type

LLM Node

### Purpose

Generate Oracle Fusion AP Invoice JSON payload

---

## Step 5 – Create Invoice

### Node Type

Business Object Function

### Oracle API

POST /invoices

### Purpose

Create AP Invoice in Oracle Fusion

---

# Non-PO Invoice Flow

## Step 1 – Fetch Supplier Details

### Node Type

Business Object Function

### Purpose

Retrieve supplier information

---

## Step 2 – Fetch Supplier Sites

### Node Type

Business Object Function

### Purpose

Retrieve:

* Supplier Sites
* Procurement Business Units

---

## Step 3 – AI Supplier Site Validation

### Node Type

LLM Node

### Purpose

AI evaluates:

* supplier site combinations
* procurement business units

---

## Step 4 – Human Approval

### Node Type

Human Approval Node

### Purpose

User confirms:

* supplier site
* business unit

---

## Step 5 – Build Non-PO Payload

### Node Type

LLM Node

### Purpose

Generate Oracle Fusion AP Invoice payload

---

## Step 6 – Create Invoice

### Node Type

Business Object Function

### Oracle API

POST /invoices

---

# Final Confirmation Layer

## Node Type

LLM + Email

### Purpose

Generate:

* business-friendly summaries
* invoice confirmation messages
* operational outputs

---

# AI Design Principles

## Deterministic AI Responses

Prompts enforce:

* JSON-only outputs
* no markdown
* no hallucinated data
* strict formatting

---

# Human-in-the-Loop Governance

Human review added for:

* ambiguous supplier sites
* multiple business unit mappings
* operational governance

---

# Oracle Fusion APIs Used

| API                             | Purpose            |
| ------------------------------- | ------------------ |
| suppliers                       | Supplier retrieval |
| suppliers/{id}/child/sites      | Supplier sites     |
| purchaseOrders                  | PO retrieval       |
| purchaseOrders/{id}/child/lines | PO line retrieval  |
| invoices                        | Invoice creation   |

---

# Enterprise Design Goals

The architecture focuses on:

* AI-assisted ERP automation
* deterministic payload generation
* scalable workflow orchestration
* ERP-safe AI usage
* operational governance
* reusable enterprise AI patterns

---

# Future Enhancements

* Confidence scoring
* Duplicate invoice detection
* Autonomous approvals
* AI anomaly detection
* Invoice hold automation
* Multi-agent orchestration
* Invoice policy validation
