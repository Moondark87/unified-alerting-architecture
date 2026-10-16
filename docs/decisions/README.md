# Architecture Decision Records

Key design decisions, recorded in a lightweight ADR format: context, decision, rationale, alternatives and consequences.

| ADR | Decision | Status |
|---|---|---|
| [001](001-chat-server-as-message-bus.md) | Use the existing self-hosted chat server as a lightweight message bus and routing layer | Accepted |
| [002](002-api-sms-instead-of-email-to-sms.md) | Replace carrier email-to-SMS with API-driven SMS delivery | Accepted |
| [003](003-pattern-matching-for-routing.md) | Route alerts with a structured, human-readable message pattern | Accepted |

The alert format required by ADR 003 is specified in [alert-format.md](../alert-format.md).
