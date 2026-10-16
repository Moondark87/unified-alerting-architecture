# Validation Suite

Test scenarios used to verify the alerting pipeline: routing accuracy, delivery reliability, correlation behavior and resilience when components fail.

| Document | Covers |
|---|---|
| [end-to-end-scenarios.md](end-to-end-scenarios.md) | Full-pipeline scenarios across routing, delivery, escalation and correlation |
| [routing-tests.md](routing-tests.md) | Pattern extraction, routing decisions and fallback logic |
| [delivery-tests.md](delivery-tests.md) | SMS delivery, broadcast, retries, error handling and duplicate suppression |
| [correlation-tests.md](correlation-tests.md) | SIEM correlation, severity escalation and noise suppression |
| [failure-mode-tests.md](failure-mode-tests.md) | Behavior when the chat server, SMS provider, SIEM or a monitoring source is unavailable |
| [test-matrix.md](test-matrix.md) | Every scenario on one page: severity × site × source × expected delivery path |
| [how-to-run-validation.md](how-to-run-validation.md) | How to run the scenarios against a test environment |

## What the suite confirms

- Routing decisions are predictable for every severity and site combination
- Delivery is observable: every success and failure is logged
- Correlation escalates real incidents and suppresses noise
- Failures degrade gracefully instead of failing silently
- Every event can be traced end to end in the SIEM
