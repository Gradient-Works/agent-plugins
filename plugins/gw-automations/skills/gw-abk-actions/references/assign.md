# Assign

Direct assignment actions.

## Directly Assign Items

**Action class:** `DirectAssignSingleItemAction`

Assigns one or more items directly to a specific User or Salesforce Queue.

For User assignments, defaults to OwnerId. Set `itemField` to a different User lookup
field API name if you need to assign via another field. Queue assignments always use OwnerId.

Set `updateImmediately` to false to perform the assignment without saving the changes immediately —
useful when bulkifying flows that use a separate Update Records step.

Use the `assignment` or `assignments` output to get details about what was assigned and to
whom. Assignment objects can be passed as input to notification actions such as
Send Single Assignment Email or Send Slack Message.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `assigneeId` | `Id` | No | Id of the User or Queue to assign to. Provide one of assigneeId, assigneeRecord, or assigneeName. |
| `assigneeName` | `String` | No | Queue name or User/Queue Id string. Provide one of assigneeId, assigneeRecord, or assigneeName. If a name is provided, the Queue must exist and have a unique name. |
| `assigneeRecord` | `SObject` | No | User or Queue record to assign to. Provide one of assigneeId, assigneeRecord, or assigneeName. |
| `assignmentDetails` | `String` | No | Optional notes to record with this assignment. |
| `item` | `SObject` | No | The record to assign. Provide item or items — not both. |
| `itemField` | `String` | No | The field on the item to use for assignment. Defaults to OwnerId. Only applies to User assignments — Queue assignments always use OwnerId. |
| `items` | `List<SObject>` | No | The records to assign. Provide item or items — not both. |
| `updateImmediately` | `Boolean` | No | Set to false to perform the assignment without saving the changes immediately. Defaults to true. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignment` | `Assignment` | The first assignment result. Can be used when working with a single item in place of results. |
| `assignments` | `List<Assignment>` | The results of all item assignments. |

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
