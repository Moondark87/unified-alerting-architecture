# Examples

Sanitized sample payloads showing one alert moving through each layer of the pipeline. Hostnames, users, device IDs and alert IDs are placeholders.

Most files follow a single incident, `itim-784512` (database unreachable at SITE-A), from webhook to SIEM.

| Stage | File | Shows |
|---|---|---|
| 1. Ingest | [itim-webhook.json](itim-webhook.json) | Monitoring (ITIM) webhook event |
| 1. Ingest | [endpoint-mgmt-webhook.json](endpoint-mgmt-webhook.json) | Endpoint management agent-health event |
| 2. Route | [chat-ingestion-message.md](chat-ingestion-message.md) | How the alert appears in the ingestion channel |
| 2. Route | [routing-patterns.md](routing-patterns.md) | Message patterns and how fields drive routing |
| 2. Route | [endpoint-mgmt-routing-example.json](endpoint-mgmt-routing-example.json) | Routing decision for a MEDIUM, chat-only alert |
| 3. Deliver | [sms-api-payload.json](sms-api-payload.json) | Request sent to the SMS API |
| 3. Deliver | [mass-alert-broadcast.json](mass-alert-broadcast.json) | Company-wide broadcast payload |
| 4. Escalate | [escalation-chain.md](escalation-chain.md) | Time-based escalation by severity |
| 5. Log | [siem-log-ingest.json](siem-log-ingest.json) | Successful delivery record |
| 5. Log | [delivery-failure.json](delivery-failure.json) | Failed delivery record (`CARRIER_UNAVAILABLE`) |
| 5. Correlate | [siem-correlated-alert.json](siem-correlated-alert.json) | ITIM + endpoint events correlated into one CRITICAL incident |
| 5. Correlate | [itim-siem-correlation-example.json](itim-siem-correlation-example.json) | HIGH latency alert escalated to CRITICAL by related SIEM logs |

Field definitions are in [docs/alert-format.md](../docs/alert-format.md).
