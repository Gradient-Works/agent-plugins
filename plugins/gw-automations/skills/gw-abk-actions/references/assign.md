# Assign

Direct assignment actions.

## Directly Assign Items

Use this action to assign one or more items directly to a specific User or
a Salesforce Queue.

For assignments to Users, we default to using the OwnerId field. If you want to use
a different field for assignments to a User, specify that value for the
assignment field in the editor.
This must be a User lookup field on the item being assigned.

Assignments to Salesforce Queues will always use the OwnerId field.

By default, the assignment is updated immediately in the database. If you
want to perform the assignments but delay the update (helpful in bulkifying
flows), change Update Item in the editor to "Later, Using Update Records".
This will set `updateImmediately` to false.

Use the `assignment` or `assignments` output to see information about the
assignments that were performed as part of a Flow run. You can use the individual
Assignment objects as an input to other actions such as
SendSingleEmailNotificationAction.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `assigneeId` | `Id` | No | The Id of the User or Salesforce Queue to assign to. |
| `assigneeName` | `String` | No | Salesforce Queue name, or a User/Queue Id string. Use when assigneeId and assigneeRecord are not available. If a name is provided, the Queue must exist and have a unique name. |
| `assigneeRecord` | `SObject` | No | User or Salesforce Queue Record to assign item to. |
| `assignmentDetails` | `String` | No | Any additional details about the assignment. |
| `item` | `SObject` | No | The Salesforce object to assign if assigning a single item. |
| `itemField` | `String` | No | The field on the item to use for User assignment. Defaults to OwnerId. |
| `items` | `List<SObject>` | No | The List of Salesforce objects to assign. |
| `updateImmediately` | `Boolean` | No | Set to false to perform the assignment but not to update the item record in the database. Defaults to true. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignment` | `Assignment` | The first assignment result. Can be used when working with a single item in place of results. |
| `assignments` | `List<Assignment>` | The results of all item assignments. |

**`Assignment` fields:**
Represents the result of a single item assignment. Produced by Directly Assign Items
and Queue-based assignment actions. Can be passed as input to notification actions
such as Send Single Email Notification.

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
