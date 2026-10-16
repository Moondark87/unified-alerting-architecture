# Correlation Engine Test Scenarios

These tests validate how SIEM correlates events from multiple systems and escalates severity.

---

## 1. ITIM + Endpoint Correlation

### Input
- ITIM: CRITICAL database alert
- Endpoint Platform: multiple agent failures

### Expected Behavior
- SIEM correlates events
- Severity escalates to CRITICAL (if not already)
- Routing layer receives correlated alert
- Correlation ID assigned

---

## 2. ITIM + Network Anomalies

### Input
- ITIM: HIGH API latency
- SIEM: packet loss on WAN link

### Expected Behavior
- Correlation engine identifies shared service group
- Severity escalates
- Routed to SITE-HQ-ONCALL

---

## 3. Multi-Site Correlation

### Input
- SITE-A: service outage
- SITE-B: similar outage
- HQ: API failures

### Expected Behavior
- Correlation engine identifies widespread impact
- SITE=ALL assigned
- Mass broadcast triggered

---

## 4. False Positive Filtering

### Input
- Endpoint Platform: repeated agent check-in delays
- No supporting ITIM or SIEM events

### Expected Behavior
- Correlation engine suppresses noise
- Severity downgraded
- Chat-only notification

---

## 5. Security + Infrastructure Correlation

### Input
- SIEM: suspicious authentication failures
- ITIM: service degradation on same host

### Expected Behavior
- Correlation engine flags potential security-linked outage
- Severity escalates
- Routed to NOC/SOC + Engineering

---

## 6. Correlation Failure Mode

### Scenario
SIEM correlation engine offline

### Expected Behavior
- Alerts routed individually
- No severity escalation
- SIEM logs correlation engine outage
