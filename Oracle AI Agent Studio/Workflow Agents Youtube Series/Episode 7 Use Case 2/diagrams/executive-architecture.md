```mermaid
flowchart LR

User[Business User]

Agent[Oracle AI Agent Studio]

LLM[LLM Services]

HCM[Oracle Fusion HCM]

BIP[BI Publisher]

Projects[Oracle Fusion Projects]

User --> Agent

Agent --> LLM

Agent --> HCM

Agent --> BIP

Agent --> Projects

HCM --> Agent

BIP --> Agent

Projects --> Agent

LLM --> Agent

Agent --> User
```
