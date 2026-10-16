# ADR 002: Replacement of Email-to-SMS with API-Driven SMS Delivery
Author: Alex Scheaffer

Status: Accepted
Date: 2025-06-17 (Sanitized)

## Context
AT&T discontinued email-to-SMS delivery, causing all SMS alerts from Nagios, SIEM, and Atera to fail. Email-to-SMS was already unreliable, lacked delivery confirmation, and was dependent on carrier-specific behavior.

A replacement was required that:

- restored SMS alerting immediately  
- provided delivery confirmation  
- worked across all carriers  
- supported structured routing logic  
- eliminated dependency on email infrastructure  

## Decision
Replace email-to-SMS with a direct, API-driven SMS delivery service.

## Rationale
- **Carrier independence:** API delivery works regardless of carrier changes.  
- **Delivery confirmation:** Monitoring teams can verify whether alerts were delivered.  
- **Structured payloads:** Alerts can include metadata (severity, location, system).  
- **Scalable:** Supports mass alerts and targeted groups.  
- **Reliable:** No spam filtering, rate limiting, or email delays.  
- **Future-proof:** Can integrate with voice, push notifications, or other channels.

## Alternatives Considered
- **Switching to another carrier’s email-to-SMS:** Temporary fix; same fragility.  
- **Using chat-only alerts:** Insufficient for on-call responders who rely on SMS.  
- **Building an internal SMS gateway:** High operational cost and maintenance burden.

## Consequences
- Requires API credentials and secure storage.  
- Monitoring tools must send webhooks instead of emails.  
- Delivery logs must be ingested into SIEM for auditability.

## Status
Implemented and validated in production.
