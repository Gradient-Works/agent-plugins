# Logs

Actions for writing messages to the Gradient Works log, useful for debugging flow executions.

## Log Message

**Action class:** `LogAction`

Publish a message to the Gradient Works Log. This is useful for debugging
your Flows.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `level` | `Integer` | Yes | The Salesforce LoggingLevel ordinal for the message: 1=ERROR, 2=WARN, 3=INFO, 4=DEBUG, 5=FINE, 6=FINER, 7=FINEST. |
| `message` | `String` | Yes | The message to log. Flow merge fields are resolved before the message is published. |
| `flowInterviewGuid` | `String` | No | Set this to `$Flow.InterviewGuid`. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `message` | `String` | The message published to the log. |

---
