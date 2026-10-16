# Escalation Chain Example

Escalation logic is based on severity and time-to-acknowledge.

## Severity: CRITICAL
1. **Immediate:** SITE-A-ONCALL (SMS + Chat)
2. **+5 minutes (no ack):** NOC/SOC (SMS + Chat)
3. **+10 minutes (no ack):** Engineering Leadership (Chat)
4. **+15 minutes (no ack):** Executive Notification (Chat)

## Severity: HIGH
1. SITE-A-ONCALL (Chat)
2. NOC/SOC (+10 minutes)

## Severity: MEDIUM
- Chat only  
- No escalation unless repeated 3 times in 30 minutes

## Notes
- Acknowledgement resets the chain.
- All escalation events are logged to SIEM.
