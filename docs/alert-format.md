# Alert Format & Routing Specification

This document defines the alert format required by [ADR 003](decisions/003-pattern-matching-for-routing.md) and the routing rules applied to it. Every monitoring source must emit alerts that conform to this format, either natively or through its webhook template.

---

## 1. Message Pattern

```
[ALERT] [SITE=<site>] [SEV=<severity>] <human-readable message>
```

| Token | Required | Description |
|---|---|---|
| `[ALERT]` | Yes | Marks the message as a routable alert. Messages without it are ignored by the router. |
| `[SITE=<site>]` | Expected | Location used to select the routing group. |
| `[SEV=<severity>]` | Expected | Severity used to select delivery channel and escalation path. |
| `<message>` | Yes | Free text, preserved verbatim for responders. |

Examples:

```
[ALERT] [SITE=SITE-A] [SEV=CRITICAL] Database unreachable for 3 checks
[ALERT] [SITE=SITE-B] [SEV=HIGH] Packet loss detected on core switch
[ALERT] [SITE=HQ] [SEV=MEDIUM] Endpoint agent check-in delayed
[ALERT] [SITE=ALL] [SEV=CRITICAL] Major outage impacting multiple locations
```

### Allowed values

| Field | Values |
|---|---|
| `SITE` | A configured site code (e.g. `SITE-A`, `SITE-B`, `HQ`) or `ALL` for company-wide events |
| `SEV` | `CRITICAL`, `HIGH`, `MEDIUM` |

---

## 2. Webhook Payload

Sources that post JSON send the same information as structured fields. The router builds the message pattern from these fields before posting to the ingestion channel.

| Field | Required | Example |
|---|---|---|
| `source` | Yes | `itim`, `siem`, `endpoint_mgmt_platform` |
| `event_type` | Yes | `service_alert`, `agent_health` |
| `alert_id` | Yes | `itim-784512` |
| `timestamp` | Yes | ISO 8601 UTC, e.g. `2026-01-14T03:22:11Z` |
| `site` | Expected | `SITE-A` |
| `severity` | Expected | `CRITICAL` |
| `message` | Yes | `Database unreachable for 3 consecutive checks` |
| `host`, `service`, `device_id`, `state` | Optional | Source-specific context |

See [`examples/itim-webhook.json`](../examples/itim-webhook.json) and [`examples/endpoint-mgmt-webhook.json`](../examples/endpoint-mgmt-webhook.json).

---

## 3. Routing Rules

### By severity

| Severity | Delivery channel | Escalation |
|---|---|---|
| `CRITICAL` | SMS + Chat | +5 min NOC/SOC, +10 min Engineering Leadership, +15 min Executive |
| `HIGH` | Chat | +10 min NOC/SOC |
| `MEDIUM` | Chat only | None, unless repeated 3 times in 30 minutes |

### By site

| Condition | Routing group |
|---|---|
| `SITE=<code>` | `<code>-ONCALL` (e.g. `SITE-A-ONCALL`) |
| `SITE=ALL` + `CRITICAL` | Company-wide broadcast: all on-call groups + leadership, SMS and chat |

### Fallbacks

| Condition | Result |
|---|---|
| `SEV` missing | Routed to `DEFAULT-ONCALL` via chat, classified **Unclassified Alert**, anomaly logged to SIEM |
| `SITE` missing, `SEV=CRITICAL` | Routed to `GLOBAL-ONCALL` via SMS + chat |
| Payload malformed (invalid JSON, missing required fields) | Routed to `DEFAULT-ONCALL`, classified `MALFORMED_ALERT`; no SMS unless severity can be inferred |

### Noise controls

| Control | Rule |
|---|---|
| Duplicate suppression | Identical alert within 60 seconds is suppressed and logged |
| Rate limiting | 100+ alerts from one source in 60 seconds are grouped into a single summary message |
| Acknowledgement | Acknowledging an alert stops its escalation chain |

---

## 4. Logging Requirements

Every stage writes to the SIEM so the full path of an alert can be reconstructed:

| Stage | Logged |
|---|---|
| Ingestion | Raw event, source, alert ID |
| Routing | Parsed site/severity, selected group and channel, classification |
| Delivery | Channel, group, status (`DELIVERED` / `FAILED`), error code, timestamp |
| Escalation | Each escalation step and acknowledgement |
| Correlation | Correlation ID, contributing events, final severity |

See [`examples/siem-log-ingest.json`](../examples/siem-log-ingest.json) and [`examples/delivery-failure.json`](../examples/delivery-failure.json).
