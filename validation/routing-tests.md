# Routing Logic Test Scenarios

These tests validate that the routing layer correctly interprets alert patterns, extracts metadata, and selects the appropriate delivery path.

---

## 1. Critical Infrastructure Alert

### Input
```
[ALERT] [SITE=SITE-A] [SEV=CRITICAL] Database unreachable for 3 checks
```

### Expected Routing
- Delivery Channel: SMS + Chat
- Group: SITE-A-ONCALL
- Escalation: Enabled (5/10/15 min)
- SIEM Logging: Yes

### Validation
- Confirm pattern extraction (SITE-A, CRITICAL)
- Verify SMS API call
- Confirm chat ingestion message
- Validate SIEM log entry

---

## 2. High-Severity Network Alert

### Input
```
[ALERT] [SITE=SITE-B] [SEV=HIGH] Packet loss detected on core switch
```

### Expected Routing
- Delivery Channel: Chat
- Group: SITE-B-ONCALL
- Escalation: Enabled (10 min)
- SIEM Logging: Yes

---

## 3. Medium-Severity Endpoint Alert

### Input
```
[ALERT] [SITE=HQ] [SEV=MEDIUM] Endpoint agent check-in delayed
```

### Expected Routing
- Delivery Channel: Chat only
- Group: HQ-ONCALL
- Escalation: None
- SIEM Logging: Yes

---

## 4. Missing Severity Field

### Input
```
[ALERT] [SITE=SITE-A] API latency above threshold
```

### Expected Routing
- Delivery Channel: Chat
- Group: DEFAULT-ONCALL
- Classification: “Unclassified Alert”
- SIEM Logging: Yes (anomaly)

---

## 5. Missing Site Field

### Input
```
[ALERT] [SEV=CRITICAL] Service outage detected
```

### Expected Routing
- Delivery Channel: SMS + Chat
- Group: GLOBAL-ONCALL
- SIEM Logging: Yes

---

## 6. Company-Wide Broadcast

### Input
```
[ALERT] [SITE=ALL] [SEV=CRITICAL] Major outage impacting multiple locations
```

### Expected Routing
- Delivery Channel: SMS Broadcast + Chat Broadcast
- Groups: All on-call + leadership
- SIEM Logging: Yes
