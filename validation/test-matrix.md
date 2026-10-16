# Test Matrix

All validation scenarios on one page. IDs map to the detailed test documents.

**Prefixes:** `RT` [routing](routing-tests.md) · `DL` [delivery](delivery-tests.md) · `CR` [correlation](correlation-tests.md) · `FM` [failure mode](failure-mode-tests.md)

## Routing

| ID | Source | Site | Severity | Expected group | Channel | Escalation |
|---|---|---|---|---|---|---|
| RT-1 | ITIM | SITE-A | CRITICAL | SITE-A-ONCALL | SMS + Chat | 5 / 10 / 15 min |
| RT-2 | ITIM | SITE-B | HIGH | SITE-B-ONCALL | Chat | 10 min |
| RT-3 | Endpoint Mgmt | HQ | MEDIUM | HQ-ONCALL | Chat | None |
| RT-4 | ITIM | SITE-A | *(missing)* | DEFAULT-ONCALL | Chat | None (Unclassified, anomaly logged) |
| RT-5 | ITIM | *(missing)* | CRITICAL | GLOBAL-ONCALL | SMS + Chat | 5 / 10 / 15 min |
| RT-6 | ITIM | ALL | CRITICAL | All on-call + leadership | SMS + Chat broadcast | Broadcast |

## Delivery

| ID | Condition | Expected status | Retry | SIEM record |
|---|---|---|---|---|
| DL-1 | Provider accepts message | DELIVERED | No | Delivery log |
| DL-2 | `CARRIER_UNAVAILABLE` | FAILED | Yes (if enabled) | Failure + retry |
| DL-3 | `INVALID_DESTINATION` | FAILED | No | "Invalid Recipient" |
| DL-4 | Broadcast payload | Aggregated | Per recipient | Total / success / failure counts + correlation ID |
| DL-5 | MEDIUM alert | No SMS sent | n/a | Routing decision |
| DL-6 | Duplicate within 60 s | Suppressed | n/a | Suppression event |

## Correlation

| ID | Inputs | Expected outcome |
|---|---|---|
| CR-1 | ITIM CRITICAL + multiple endpoint failures | Correlated, CRITICAL, correlation ID assigned |
| CR-2 | ITIM HIGH latency + WAN packet loss | Escalated, routed to SITE-HQ-ONCALL |
| CR-3 | Outages at SITE-A, SITE-B and HQ | SITE=ALL, mass broadcast |
| CR-4 | Repeated endpoint delays, no supporting events | Suppressed, downgraded, chat only |
| CR-5 | Auth failures + degradation on same host | Escalated to NOC/SOC + Engineering |
| CR-6 | Correlation engine offline | Alerts routed individually, outage logged |

## Failure modes

| ID | Failed component | Expected behavior |
|---|---|---|
| FM-1 | Chat server | Degraded mode; SMS routed directly; `CHAT_UNAVAILABLE` logged |
| FM-2 | SMS provider | Retries; chat fallback for CRITICAL; outage logged |
| FM-3 | SIEM | Routing continues; logs buffered locally and synced on recovery |
| FM-4 | Malformed payload | DEFAULT-ONCALL; `MALFORMED_ALERT`; no SMS unless severity inferable |
| FM-5 | Endpoint flood (100+ in 60 s) | Rate limiting; summary message; no SMS spam |
| FM-6 | ITIM outage | Heartbeat alert to NOC/SOC; escalation enabled |
