# SMS Delivery Replacement Proposal
Author: Alex Scheaffer

Status: Implemented (Sanitized Version)

## Overview
AT&T discontinued support for email-to-SMS delivery, which immediately broke all text-based alerting for Nagios, SIEM, and Atera. This proposal outlines a replacement architecture using API-driven SMS delivery to restore and improve alert reliability.

## Problem Statement
- AT&T disabled email-to-text, causing all SMS alerts to fail.
- Nagios, SIEM, and Atera relied heavily on SMS for critical notifications.
- No fallback mechanism existed.
- On-call teams were unaware of outages due to silent failures.
- Email-to-SMS was inherently unreliable and lacked delivery confirmation.

## Requirements
- Restore SMS alerting for all monitoring systems.
- Replace email-to-SMS with a modern, API-based solution.
- Provide delivery confirmation and logging.
- Support location-based and team-based routing.
- Integrate seamlessly with existing monitoring tools.
- Ensure long-term carrier independence.

## Proposed Architecture
### API-Driven SMS Delivery
- Replace email-to-SMS with a direct SMS API (Twilio or equivalent).
- Monitoring tools send webhook events to the routing layer.
- Routing layer formats and forwards messages to the SMS API.

### Routing Logic
- Severity-based routing (Critical, High, Medium).
- Location-based groups (Site A, Site B, etc.).
- Team-based escalation (On-call, NOC/SOC, Engineering).

### Delivery Logging
- All SMS delivery attempts logged and forwarded to SIEM.
- Provides visibility into:
  - Delivery success/failure
  - Timestamp
  - Recipient group
  - Message content

### Integration Points
- Nagios → Webhook → Routing Layer → SMS API  
- SIEM → Correlated Alerts → Routing Layer → SMS API  
- Atera → Webhook → Routing Layer → SMS API  

## Expected Outcomes
- Restored SMS alerting with higher reliability than email-to-SMS.
- Delivery confirmation ensures no silent failures.
- Carrier independence prevents future disruptions.
- Improved routing logic reduces noise and increases relevance.
- Full auditability through SIEM integration.

## Summary
This proposal replaced a fragile, carrier-dependent alerting mechanism with a modern, API-driven SMS delivery system. It restored operational visibility, improved reliability, and established a scalable foundation for future alerting enhancements.
