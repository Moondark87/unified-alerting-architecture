# Delivery Layer Test Scenarios

These tests validate SMS delivery, broadcast behavior, retry logic, and error handling.

---

## 1. Successful SMS Delivery

### Input
SMS API payload for SITE-A-ONCALL

### Expected Behavior
- SMS provider returns status: DELIVERED
- Delivery logged to SIEM
- No retry triggered

---

## 2. Carrier Unavailable

### Input
SMS provider returns error: CARRIER_UNAVAILABLE

### Expected Behavior
- Delivery logged as FAILED
- Retry logic (if enabled) triggers
- SIEM logs failure + retry attempt

---

## 3. Invalid Phone Number

### Input
SMS provider returns error: INVALID_DESTINATION

### Expected Behavior
- Delivery logged as FAILED
- No retry (invalid target)
- SIEM logs classification: “Invalid Recipient”

---

## 4. Broadcast Delivery

### Input
Mass alert broadcast payload

### Expected Behavior
- SMS sent to all configured groups
- Delivery results aggregated
- SIEM logs:
  - total recipients
  - success count
  - failure count
  - correlation ID

---

## 5. Chat Delivery Only

### Input
Medium severity alert

### Expected Behavior
- No SMS API call
- Chat message posted
- SIEM logs routing decision

---

## 6. Duplicate Alert Suppression

### Input
Same alert received twice within 60 seconds

### Expected Behavior
- Second alert suppressed
- SIEM logs suppression event
- No duplicate SMS
