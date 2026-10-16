# Routing Pattern Examples

The routing layer uses predictable, human-readable patterns to determine severity, location, and routing rules.

## Example 1: Critical Site Outage
```
[ALERT] [SITE=SITE-A] [SEV=CRITICAL] Database unreachable for 3 checks
```

## Example 2: High-Severity Network Issue
```
[ALERT] [SITE=SITE-B] [SEV=HIGH] Packet loss detected on core switch
```

## Example 3: Medium-Severity Application Warning
```
[ALERT] [SITE=HQ] [SEV=MEDIUM] API latency above threshold
```

## Example 4: Company-Wide Broadcast
```
[ALERT] [SITE=ALL] [SEV=CRITICAL] Major outage impacting multiple locations
```

## Pattern Rules
- `SITE=` determines location-based routing groups  
- `SEV=` determines escalation path  
- Message body is preserved for human readability
