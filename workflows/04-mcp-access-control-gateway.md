# Workflow 4 — MCP Server Access Control Gateway

## What It Does

A gateway proxy sits between AI agents and all Model Context Protocol servers, enforcing authentication, allowlisting, rate limiting, and logging on every tool invocation.

---

## Security Controls Applied

| Control | Implementation |
|---------|---------------|
| Server allowlist | Agents can only connect to pre-approved MCP servers |
| Per-agent permissions | Each agent has its own approved server + tool list |
| Request signing | All MCP requests carry a signed JWT with agent identity |
| Response validation | MCP responses validated against schemas before the agent receives them |
| Rate limiting | Per agent, per MCP server |
| Full request logging | Request + response metadata logged at the gateway layer |

---

## Architecture

```mermaid
graph TD
    A[AI Agent] --> B[MCP Gateway / Proxy]
    B --> C{Allowlist Check}
    C -->|Not allowed| D[Block + Log]
    C -->|Allowed| E{Auth Check}
    E -->|Invalid| F[Reject + Alert]
    E -->|Valid| G{Rate Limit Check}
    G -->|Exceeded| H[429 + Log]
    G -->|OK| I[Route to MCP Server]

    I --> J[github-mcp]
    I --> K[slack-mcp]
    I --> L[database-mcp]

    J --> N[Response Validator]
    K --> N
    L --> N

    N -->|Valid| O[Return to Agent]
    N -->|Invalid schema| P[Block + Alert]

    B --> Q[Audit Logger]

    style B fill:#99ccff
    style C fill:#ffcc99
    style N fill:#ffcc99
```

---

## Configuration

See [`configs/mcp-gateway.yaml`](../configs/mcp-gateway.yaml) for the full gateway configuration.

---

*Next: [Workflow 5 — Multi-Agent Zero Trust →](05-multi-agent-zero-trust.md)*
