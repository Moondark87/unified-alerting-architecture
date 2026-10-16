# Unified Alerting & Notification Architecture  
Author: Alex Scheaffer

Document: Architecture Explanation (Sanitized)

## Overview

This document explains the unified alerting and notification architecture in a layered, systems-focused way. The design solves two core problems:

- Lack of mass alerting for widespread or site-down incidents  
- Loss of email-to-SMS delivery after AT&T disabled that capability

The architecture creates a structured pipeline from monitoring events to human recipients, with full logging and auditability at every stage.

See the [architecture diagram](../README.md#architecture) and the [alert format specification](alert-format.md).

---

## Layer 1: Monitoring & Alerting Layer

**Primary Role:** Generate signals and define what constitutes an alert.

**Components:**

- **Monitoring Systems (NMS / Nagios / Atera / SIEM):**  
  These systems perform checks on infrastructure, applications, and services. They detect outages, threshold breaches, and anomalies.

- **Thresholds & Outage Checks:**  
  Rules that determine when an event becomes an alert (for example, service down, latency above X, error rate above Y).

- **Webhook Event Triggers:**  
  Instead of relying on email, monitoring tools send structured events (JSON or similar) to the routing layer via webhooks.

**Why this layer matters:**  
It standardizes how alerts are generated and ensures that downstream systems receive machine-readable, structured events instead of ad-hoc emails.

---

## Layer 2: Communication & Routing Layer

**Primary Role:** Interpret, classify, and route alerts based on content, severity, and location.

**Components:**

- **Self-Hosted Chat Server (e.g., Mattermost / similar):**  
  - **Alert Ingestion Channel:**  
    A dedicated channel where all incoming alerts are posted in a consistent format.  
  - **Pattern-Matched Routing:**  
    Alerts are formatted with predictable patterns (for example, `[ALERT] [SITE=XYZ] [SEV=CRITICAL] Message`).  
    These patterns are used to determine routing rules.  
  - **Location / Severity Logic:**  
    The system parses the alert content to decide which teams, locations, or groups should receive notifications.

- **Message Bus Function:**  
  The chat system effectively acts as a lightweight message bus:
  - Provides a human-readable audit trail  
  - Allows internal collaboration on alerts  
  - Serves as a central point for routing decisions

**Why this layer matters:**  
It decouples monitoring tools from delivery mechanisms. Instead of each tool sending alerts directly to recipients, they send to a central routing layer that can evolve independently.

---

## Layer 3: Notification Delivery Layer

**Primary Role:** Deliver alerts to humans through reliable, modern channels.

**Components:**

- **Text / SMS Delivery Service (API-based):**  
  - **API-Driven Messaging:**  
    Replaces fragile email-to-SMS with direct API calls to an SMS provider.  
  - **Location-Based Groups:**  
    Alerts can be sent to specific sites, regions, or functional groups.  
  - **Company-Wide Alerts:**  
    Supports mass broadcast for major incidents or site-wide outages.  
  - **Delivery Logging:**  
    Every SMS attempt is logged, including success/failure, timestamp, and target group.

**Why this layer matters:**  
It restores and improves SMS alerting after the AT&T change, and it provides a scalable, carrier-independent delivery mechanism for both routine and mass alerts.

---

## Layer 4: Recipient Layer

**Primary Role:** Receive and act on alerts.

**Recipients:**

- **On-Call Teams:**  
  Primary responders for infrastructure and application incidents.

- **NOC / SOC:**  
  Operations and security teams that monitor and respond to ongoing events.

- **Engineering:**  
  Product and platform engineers who may need to investigate or remediate issues.

- **Executive Notifications:**  
  Leadership visibility for major outages, customer-impacting events, or compliance-related incidents.

**Why this layer matters:**  
It formalizes who gets what, and when. Instead of ad-hoc distribution, the architecture encodes which roles and groups are notified based on severity and scope.

---

## Layer 5: Logging & Intelligence Layer

**Primary Role:** Provide observability, auditability, and long-term insight into alerting behavior.

**Components:**

- **SIEM / Log Platform:**
  - **Ingests Monitoring Events:**  
    Raw alerts and events from monitoring tools.
  - **Ingests Chat Messages:**  
    Alert posts, routing decisions, and human commentary.
  - **Ingests SMS Delivery Logs:**  
    Delivery attempts, status, and metadata.
  - **Correlation & Retention:**  
    Enables correlation between alerts, responses, and outcomes over time.

**Why this layer matters:**  
It turns the alerting system into a measurable, auditable pipeline. You can answer questions like:
- Did the right people get notified?  
- How long did it take?  
- Which alerts are noisy or misrouted?  
- Where are failures occurring in the pipeline?

---

## End-to-End Flow Summary

1. **Monitoring tools detect an issue** and trigger a webhook event.  
2. **The routing layer receives the event**, posts it into a chat ingestion channel, and applies pattern-based logic to determine severity and destination.  
3. **The notification layer sends SMS (and potentially other channels)** to the appropriate groups, with delivery logging.  
4. **Recipients act on the alert**, using chat for coordination and incident handling.  
5. **All events, messages, and delivery logs are ingested into the SIEM**, creating a complete, correlated record of detection, notification, and response.

---

## Design Principles

- **Decoupling:** Monitoring, routing, and delivery are separated so each can evolve independently.  
- **Auditability:** Every step is logged in a human-readable and machine-readable way.  
- **Resilience:** The system is not tied to a single carrier or email-to-SMS behavior.  
- **Scalability:** New tools, channels, and groups can be added without redesigning the core flow.  
- **Clarity:** Alerts follow a consistent format, making routing and human interpretation easier.

---

## Summary

This architecture replaces fragile, ad-hoc alerting with a structured, layered system that supports both routine alerts and mass notifications. It restores SMS delivery after the AT&T change, introduces a first-class mass alerting capability, and provides full visibility into how alerts move through the environment from detection to human action.
