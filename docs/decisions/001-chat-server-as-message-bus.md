# ADR 001: Selection of Chat Server as Lightweight Message Bus
Author: Alex Scheaffer

Status: Accepted
Date: 2025-06-17 (Sanitized)

## Context
The alerting pipeline required a central point where alerts from multiple monitoring systems could be ingested, normalized, routed, and audited. Traditional message bus platforms (Kafka, RabbitMQ, SNS/SQS) were considered, but the environment needed a solution that:

- supported human-readable collaboration  
- provided an audit trail  
- required minimal operational overhead  
- integrated easily with webhooks  
- allowed routing logic to be built incrementally  

A self-hosted chat platform was already in place and supported webhook ingestion, channel-based routing, and structured message formatting.

## Decision
Use the existing chat server as a lightweight message bus and routing layer for all alerting events.

## Rationale
- **Human-readable audit trail:** Every alert is visible, timestamped, and searchable.  
- **Zero additional infrastructure:** No new message bus cluster to deploy or maintain.  
- **Webhook-native:** Monitoring tools can post directly without custom adapters.  
- **Pattern-based routing:** Alerts can be parsed and routed using predictable message formats.  
- **Collaboration built-in:** Teams can discuss alerts in the same place they appear.  
- **Low latency:** Chat ingestion is effectively real-time.  
- **Extensible:** Additional routing logic can be layered on without redesign.

## Alternatives Considered
- **Kafka / RabbitMQ:** Too heavy for the environment; required new infrastructure and operational support.  
- **Direct API calls from monitoring tools:** Tight coupling, no audit trail, no collaboration.  
- **Email-based routing:** Fragile, slow, and deprecated by carriers.

## Consequences
- Chat server becomes a critical component of the alerting pipeline.  
- Routing logic must remain consistent and predictable.  
- Future integrations (Slack, Teams, SMS) can be layered on without redesign.

## Status
Implemented and validated in production.
