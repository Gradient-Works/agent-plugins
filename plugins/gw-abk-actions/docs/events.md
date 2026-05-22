# Events

Actions for handling Gradient Works platform events.

## Convert ItemAssigned to Assignment

When Gradient Works makes an assignment, it fires a Salesforce platform
event of type `ItemAssigned__e`. This action is a utility that converts
those events to an Assignment which can be used as an input to other
actions.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `event` | `ItemAssigned__e` | Yes | The event indicating an item was assigned |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignment` | `Assignment` | Information about the assignment represented by the event |

**`Assignment` fields:**
Actions such as AssignSingleItemAction and AssignMultiItemAction provide
these to communicate the results of assignment. You can also use them as
input into other actions like SendSingleEmailNotificationAction.

| Field | Type | Description |
|-------|------|-------------|
| `queue` | `String` | The Queue that was used to perform the assignment. This will either be an Id or name depending on how the Queue was specified by the action that produced this result. For Assignments that result from DirectAssignSingleItemAction this value will be null. |
| `item` | `GenericSObject` | Wraps the SObject that was assigned. Flow does not currently handle SObjects very well so this wraps an SObject in a Flow-friendly interface. |
| `assignedTo` | `User` | The User that the item was assigned to. Only the Id, Email, Name, Alias fields will be available. Value will be null if item was assigned to a standard Salesforce Queue. |
| `assignedToQueue` | `Group` | The standard Salesforce Queue that the item was assigned to. Only the Id, Email, Name fields will be available. Value will be null if item was assigned to a User. |
| `previouslyAssignedTo` | `User` | The User the item was previously assigned to. Only the Id, Email, Name, Alias fields will be available. |
| `success` | `Boolean` | Whether or not the assignment completed successfully |
| `errorMessage` | `String` | Any error that occurred during assignment |

---
