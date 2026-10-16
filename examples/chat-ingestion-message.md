# Chat Ingestion Message Example

Incoming alerts are posted into a dedicated ingestion channel on the self-hosted chat platform.

## Example Message

```
[ALERT] [SITE=SITE-A] [SEV=CRITICAL] Database unreachable for 3 checks
Source: itim
Alert ID: itim-784512
Timestamp: 2026-01-14T03:22:11Z
```

## Notes
- Message is human-readable for responders.
- Metadata is included as a threaded attachment or JSON block.
- Routing logic parses SITE and SEV fields.
