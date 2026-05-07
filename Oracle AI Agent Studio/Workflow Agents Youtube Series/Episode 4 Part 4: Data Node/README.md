# Episode 4 Part 4 — Data Node

This folder contains configurations for integrating Oracle AI Agent Studio Workflow Agents with Oracle Analytics / OTBI SOAP Web Services.

The examples demonstrate how to:

- Generate Oracle Analytics Session IDs
- Execute OTBI reports dynamically
- Consume SOAP-based analytics services inside workflow agents
- Use External REST Functions within Data Nodes

---

# Files

| File | Description |
|---|---|
| `SessionID External REST.md` | Generates Oracle Analytics Session ID using SOAP `logon` operation |
| `Run OTBI External REST.md` | Executes OTBI reports using SOAP `executeXMLQuery` operation |

---

# Workflow

```text
Generate Session ID
        ↓
Execute OTBI Report
        ↓
Parse SOAP Response
        ↓
Use Data in Workflow Agent
```

---

# Oracle Analytics SOAP Endpoint

```text
/analytics-ws/saw.dll/wsdl/v12
```

---

# Key Features

- SOAP-based OTBI integration
- Session-based authentication
- Dynamic report execution
- XML response handling
- AI Agent Studio Data Node integration

---

# Prerequisites

- Oracle Fusion / OTBI access
- Valid BI & Oracle Fusion credentials
- Access to Oracle Analytics SOAP services
- Required report permissions

---

# Notes

- Session ID generation is mandatory before report execution.
- Report paths must exactly match OTBI catalog paths.
- Preserve SOAP namespaces exactly as documented.
- Suitable for Oracle AI Agent Studio workflow automation scenarios.
