# Events

Actions for handling Gradient Works platform events.

## Convert ItemAssigned to Assignment

Deprecated. Use the assignment output from queue assignment actions directly
instead of converting ItemAssigned__e events.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `event` | `ItemAssigned__e` | Yes | The event indicating an item was assigned |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignment` | `Assignment` | Information about the assignment represented by the event |

**`Assignment` fields:**
Represents the result of a single item assignment. Can be passed as input to notification actions
such as Send Single Assignment Email or Send Slack Message.

| Field | Type | Description |
|-------|------|-------------|
| `queue` | `String` | The Gradient Works Queue used for the assignment — either an Id or name depending on how it was specified. Null for direct assignments (Directly Assign Items). |
| `item` | `GenericSObject` | The assigned record wrapped in a Flow-friendly envelope. Key fields: `id` (record Id), `name` (display name), `typeName` (e.g. "Account"), `detailUrl` (Salesforce record URL), `realSObject` (the underlying record). |
| `assignedTo` | `User` | The User that the item was assigned to. Only the Id, Email, Name, Alias fields will be available. Value will be null if item was assigned to a standard Salesforce Queue. |
| `assignedToQueue` | `Group` | The standard Salesforce Queue that the item was assigned to. Only the Id, Email, Name fields will be available. Value will be null if item was assigned to a User. |
| `previouslyAssignedTo` | `User` | The User the item was previously assigned to. Only the Id, Email, Name, Alias fields will be available. |
| `success` | `Boolean` | True if the assignment completed successfully. |
| `errorMessage` | `String` | Error message if success is false. |

---
