# Logs

Actions for writing messages to the Gradient Works log, useful for debugging flow executions.

## Log Message

Publish a message to the Gradient Works Log. This is useful for debugging
your Flows.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `level` | `Integer` | Yes | The importance level of the log message: from TRACE (lowest level) to ERROR (highest level). |
| `message` | `String` | Yes | The message to include in the log. Merge fields will be merged before publishing the log message. |
| `flowInterviewGuid` | `String` | No | A unique identifier for the running Flow "interview". This will be automatically provided. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `message` | `String` | The message published to the log |

---
