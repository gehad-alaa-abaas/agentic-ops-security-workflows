# Pre-Deployment Security Checklist

Run through every item before shipping any agent workflow to production. No exceptions.

---

## Identity & Access

- [ ] Every agent has a unique, scoped identity credential
- [ ] No shared credentials between agents or environments
- [ ] All API keys stored in a secrets manager (not env vars, not code, not config files)
- [ ] All credentials are short-lived with automatic rotation
- [ ] Least-privilege scoping applied to every credential
- [ ] Read-only access used wherever writes are not required

## Secrets Management

- [ ] No secrets appear in any log output
- [ ] No secrets appear in agent prompt context or LLM inputs
- [ ] No secrets passed between agents in message payloads
- [ ] Dynamic credentials used where possible (Vault database secrets engine)
- [ ] Credential leases are revoked at run end — including on crash

## Audit & Observability

- [ ] Every agent action produces a structured JSON log entry
- [ ] Logs written to append-only, tamper-evident store
- [ ] Logs do not contain raw PII — only hashed identifiers
- [ ] A `run_id` correlates all actions within a single agent execution
- [ ] Anomaly detection rules are active, tested, and alerting correctly
- [ ] Alerts route to the right people (Slack, PagerDuty, email)

## MCP & Tool Controls

- [ ] An allowlist of approved MCP servers defined per agent
- [ ] MCP connections require authentication (JWT, API key, or mTLS)
- [ ] MCP requests are rate-limited per agent per server
- [ ] MCP responses are schema-validated before reaching the agent
- [ ] Agent tool definitions reviewed by Skill Scanner before deploy
- [ ] Destructive tools (delete, drop, purge) explicitly excluded unless required

## Input/Output Safety

- [ ] User inputs sanitized for prompt injection before reaching the LLM
- [ ] Agent outputs scanned for PII and sensitive data before delivery
- [ ] Maximum output size limits enforced
- [ ] Human approval gates in place for high-impact, irreversible actions

## Operational Controls

- [ ] Circuit breakers exist to halt runaway agents
- [ ] Maximum run time (TTL) enforced per agent execution
- [ ] A kill switch or pause mechanism is available for all running agents
- [ ] Staging environment mirrors production security controls exactly
- [ ] Incident response runbook exists for agent-related security events

---

*If any item is unchecked, document why and get sign-off before proceeding.*
