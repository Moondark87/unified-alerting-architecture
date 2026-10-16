# ADR 003: Use of Pattern-Matching for Alert Routing
Author: Alex Scheaffer

Status: Accepted  
Date: 2025-06-17 (Sanitized)

## Context
Alerts from monitoring systems vary widely in structure, verbosity, and formatting. A routing mechanism was needed that could:

- classify alerts by severity  
- identify affected locations or systems  
- support multiple monitoring tools  
- remain human-readable  
- avoid brittle, tool-specific parsing logic  

Pattern-matching using predictable message formats (e.g., `[ALERT] [SITE=ABC] [SEV=CRITICAL] Message`) provided a universal, tool-agnostic approach.

## Decision
Use structured, pattern-based message formatting for all alerts and apply routing logic based on these patterns.

## Rationale
- **Tool-agnostic:** Works across Nagios, SIEM, Atera, and future tools.  
- **Human-readable:** Engineers can interpret alerts at a glance.  
- **Machine-parsable:** Routing logic can extract fields reliably.  
- **Low overhead:** No need for custom parsers or adapters.  
- **Consistent:** Every alert follows the same predictable structure.  
- **Extensible:** New fields (e.g., service, environment) can be added without breaking existing logic.

## Alternatives Considered
- **JSON-only routing:** Not all tools emit clean JSON; humans cannot read it easily.  
- **Regex-free keyword matching:** Too brittle and error-prone.  
- **Tool-specific routing rules:** Hard to maintain and scale.

## Consequences
- All monitoring tools must format alerts consistently.  
- Routing logic depends on predictable patterns.  
- Documentation must define required alert format.

## Status
Implemented and validated in production.
