# Validation & Test Scenarios

This document outlines validation steps used to confirm the architecture behaves correctly under normal, degraded, and failure conditions.

---

## 1. Basic Routing Validation

### Scenario
ITIM sends a CRITICAL alert for SITE-A.

### Expected Behavior
- Alert appears in chat ingestion channel.
- Pattern matching identifies SITE-A + CRITICAL.
- SMS sent to SITE-A-ONCALL.
- Delivery logged to SIEM.

### Validation Steps
- Verify chat message format.
- Confirm SMS delivery status = DELIVERED.
- Check SIEM for correlated logs.

---

## 2. Medium Severity Endpoint Alert

### Scenario
Endpoint Management Platform reports agent health warning for SITE-B.

### Expected Behavior
- Routed to SITE-B-ONCALL via chat only.
- No SMS sent.
- Logged to SIEM.

### Validation Steps
- Confirm routing decision in logs.
- Verify no SMS API call occurred.

---

## 3. Mass Alert Broadcast

### Scenario
Multiple systems report failures across multiple locations.

### Expected Behavior
- Correlation engine escalates to CRITICAL.
- Broadcast engine sends company-wide SMS.
- Chat broadcast posted.
- All events logged to SIEM.

### Validation Steps
- Confirm broadcast payload.
- Validate SMS delivery to all groups.
- Check SIEM correlation ID.

---

## 4. Delivery Failure Handling

### Scenario
SMS provider returns CARRIER_UNAVAILABLE.

### Expected Behavior
- Delivery logged as FAILED.
- Retry logic (if configured) triggers.
- SIEM logs failure event.

### Validation Steps
- Confirm failure log in SIEM.
- Validate retry attempt (if enabled).

---

## 5. Escalation Chain

### Scenario
CRITICAL alert not acknowledged within 5 minutes.

### Expected Behavior
- Escalation to NOC/SOC.
- Additional escalation at 10 and 15 minutes.
- All escalation events logged.

### Validation Steps
- Validate timestamps.
- Confirm escalation messages.
- Check SIEM for escalation chain logs.

---

## 6. Pattern Matching Validation

### Scenario
Alert missing SITE or SEV fields.

### Expected Behavior
- Routed to default group.
- Logged as “unclassified alert”.
- SIEM receives anomaly log.

### Validation Steps
- Confirm fallback routing.
- Validate SIEM anomaly entry.

---

## 7. SIEM Correlation Validation

### Scenario
ITIM alert + endpoint failures + network anomalies.

### Expected Behavior
- SIEM correlates events.
- Severity escalated.
- Routing layer receives correlated alert.

### Validation Steps
- Confirm correlation ID.
- Validate final severity.
- Check routing decision.
