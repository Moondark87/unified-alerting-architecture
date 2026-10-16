# Failure Mode Test Scenarios

These tests validate system behavior under degraded or failed conditions.

---

## 1. Chat Server Unavailable

### Expected Behavior
- Routing layer enters degraded mode
- SMS delivery still functions (direct routing)
- SIEM logs: CHAT_UNAVAILABLE
- Alerts marked as “Degraded Routing”

---

## 2. SMS Provider Outage

### Expected Behavior
- All SMS attempts fail
- Retry logic triggers (if enabled)
- Chat fallback enabled for CRITICAL alerts
- SIEM logs:
  - provider outage
  - retry attempts
  - fallback routing

---

## 3. SIEM Unavailable

### Expected Behavior
- Alerts still routed normally
- Local buffer stores logs temporarily
- SIEM sync resumes when available
- No data loss (within buffer limits)

---

## 4. Malformed Alert Payload

### Input
Missing required fields, invalid JSON, or corrupted data

### Expected Behavior
- Routed to DEFAULT-ONCALL
- Classification: MALFORMED_ALERT
- SIEM logs anomaly
- No SMS unless severity can be inferred

---

## 5. Endpoint Management Platform Flooding

### Scenario
100+ endpoint alerts in 60 seconds

### Expected Behavior
- Rate limiting activates
- Alerts grouped into summary message
- SIEM logs rate-limit activation
- No SMS spam

---

## 6. ITIM Monitoring Outage

### Expected Behavior
- Heartbeat alerts generated
- Routed to NOC/SOC
- SIEM logs monitoring outage
- Escalation chain enabled
