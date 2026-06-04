# Oracle Fusion AP Invoice Processor – Workflow Agent

AI-powered Accounts Payable Invoice Processing Workflow built using Oracle AI Agent Studio and Oracle Fusion Financials.

## Overview

This project demonstrates an end-to-end AP Invoice Processing Workflow Agent built inside Oracle AI Agent Studio.

The workflow automates invoice intake, invoice parsing, PO validation, supplier validation, invoice payload generation, and invoice creation directly within Oracle Fusion Financials using REST APIs and Workflow Agents.

The solution supports both:

* PO Matched Invoices
* Non-PO Invoices

It also includes Human-in-the-Loop approval for supplier site and business unit validation when required.

---

## Key Capabilities

* Upload Invoice PDF directly through chat
* Extract invoice data using Chat Attachment Reader
* Parse invoice information using LLM nodes
* Detect whether PO exists
* Process PO and Non-PO invoices differently
* Retrieve Supplier / Supplier Site / PO details from Oracle Fusion
* Match invoice lines against PO lines
* Human approval workflow for supplier site validation
* Dynamically generate invoice payloads
* Create AP invoices using Oracle Fusion REST APIs
* Send confirmation message/email after processing

---

# High-Level Process Flow

## Common Steps

1. Invoice PDF Uploaded
2. Extract Invoice Text
3. Parse Invoice Data using LLM
4. Check whether PO exists

---

## PO Invoice Flow

* Fetch PO Header
* Retrieve PO Supplier Site
* Fetch PO Lines
* Match Invoice Lines with PO Lines
* Build PO Invoice Payload
* Create Invoice in Oracle Fusion
* Send Final Confirmation Message

---

## Non-PO Invoice Flow

* Fetch Supplier Details
* Fetch Supplier Sites
* Validate Supplier Site using AI
* Human Review for Business Unit Selection
* Build Non-PO Invoice Payload
* Create Invoice in Oracle Fusion
* Send Final Confirmation Message

---

# Node Types Used

| Node Type            | Purpose                                                |
| -------------------- | ------------------------------------------------------ |
| Tool Node            | Read Invoice PDF attachment                            |
| LLM Node             | Parse invoice details and generate payloads            |
| Condition Node       | Check whether PO exists                                |
| Business Object Node | Fetch supplier, PO, supplier site, and create invoices |
| Human Approval Node  | Business unit/site validation                          |
| Email Node           | Send final confirmation                                |
| Variable Node        | Store PO existence flag                                |

---

# Oracle Fusion REST APIs Used

## Suppliers

GET /fscmRestApi/resources/11.13.18.05/suppliers

## Supplier Sites

GET /fscmRestApi/resources/11.13.18.05/suppliers/{SupplierId}/child/sites

## Purchase Orders

GET /fscmRestApi/resources/11.13.18.05/purchaseOrders

## Purchase Order Lines

GET /fscmRestApi/resources/11.13.18.05/purchaseOrders/{POHeaderId}/child/lines

## Create Invoice

POST /fscmRestApi/resources/11.13.18.05/invoices

---

# AI / LLM Use Cases

The workflow uses LLM nodes extensively for:

* Invoice field extraction
* JSON payload structuring
* Supplier site validation
* PO line matching
* Payload generation
* Confirmation message generation

Example AI tasks:

* Extract invoice number, amount, supplier, PO number
* Match invoice lines against PO lines using description and unit price
* Validate supplier site combinations
* Generate Oracle Fusion invoice payload dynamically

---

# Human-in-the-Loop Scenario

For Non-PO invoices, supplier sites may map to multiple Business Units.

The workflow:

1. Retrieves all supplier site combinations
2. Uses AI to summarize options
3. Sends options for human review
4. Accepts user confirmation
5. Continues invoice creation

---

# Example Technologies Used

* Oracle AI Agent Studio
* Oracle Fusion Financials
* Oracle Fusion REST APIs
* Workflow Agents
* LLM Nodes
* Human Approval Nodes
* Chat Attachment Reader
* Business Object Functions
* Communication Node

---

# Sample Workflow Architecture

The workflow contains:

* Chat Attachment Reader Tool
* Multiple LLM Nodes
* Oracle Fusion Business Object Nodes
* Human Approval Loop
* Invoice Creation APIs
* Email Notification Step

---

# Use Cases

This architecture pattern can also be extended for:

* Sales Order Creation
* Purchase Requisition Automation
* Expense Report Processing
* Supplier Onboarding
* Procurement Document Automation
* Intelligent ERP Transaction Creation

---

# Future Enhancements

Potential enhancements include:

* Multi-invoice batch processing
* OCR confidence scoring
* Duplicate invoice detection
* Invoice policy validation
* Tax validation
* Attachment archival
* Deep links to Oracle Fusion transactions
* Autonomous approval routing
* AI anomaly detection

---

# Demo Highlights

* End-to-end AP automation
* PO and Non-PO support
* Human approval integration
* Dynamic payload generation
* Oracle Fusion invoice creation
* AI-assisted ERP transaction processing

---

# Repository Contents

* Workflow JSON export
* Prompt examples
* Sample invoice PDFs
* Process flow diagrams
* Demo screenshots

---

# Disclaimer

This project is intended for learning, prototyping, and demonstration purposes around Oracle AI Agent Studio and Oracle Fusion ERP automation patterns.

---

# Author

Arpit Paliwal
Oracle ACE | Oracle ERP & AI Consultant
