# How to Run Validation

These scenarios are run against a **non-production** copy of the pipeline: a test ingestion channel, test routing groups that contain only the validating engineers, and the SMS provider's test or sandbox mode where available.

## Prerequisites

- A test ingestion channel on the chat server and a webhook URL that posts to it
- Test routing groups (`SITE-A-ONCALL`, `DEFAULT-ONCALL`, etc.) containing only test recipients
- SMS provider credentials for a test account, stored in a secrets store, never in scripts
- A SIEM search or saved query filtered to the test channel and test alert IDs

## Running a scenario

1. **Pick a scenario** from the [test matrix](test-matrix.md).
2. **Send the input.** Post the matching payload from [`examples/`](../examples/) to the test webhook, changing `alert_id` to a unique test value:

   ```bash
   curl -X POST "$TEST_WEBHOOK_URL" \
        -H "Content-Type: application/json" \
        --data @examples/itim-webhook.json
   ```

3. **Check routing.** Confirm the message appears in the ingestion channel in the standard format, and that the routing decision (group, channel, classification) matches the matrix.
4. **Check delivery.** For SMS scenarios, confirm the provider's delivery status. For chat-only scenarios, confirm that **no** SMS API call was made.
5. **Check escalation** (CRITICAL and HIGH). Leave the alert unacknowledged and confirm each escalation step fires on time, then acknowledge and confirm the chain stops.
6. **Check the SIEM.** Search by `alert_id` or `correlation_id` and confirm ingestion, routing, delivery and escalation records are all present.
7. **Record the result** (pass/fail, timestamps, notes) against the scenario ID.

## Failure-mode scenarios

Failure modes are run by deliberately taking one component out of the path in the test environment:

| Scenario | How to simulate |
|---|---|
| FM-1 Chat unavailable | Disable the test ingestion webhook |
| FM-2 SMS provider outage | Point the delivery layer at an invalid provider endpoint |
| FM-3 SIEM unavailable | Block the log forwarder, then restore it and confirm the buffer syncs |
| FM-4 Malformed payload | Send invalid JSON or remove required fields |
| FM-5 Flooding | Send 100+ test alerts from one source within 60 seconds |
| FM-6 Monitoring outage | Stop the test monitoring source's heartbeat |

Restore every component and confirm normal routing before closing a validation run.
