# Queues

Actions for enqueueing records and assigning items to users via Gradient Works queues.

## Capacity

Several queue actions accept `capacityValue` or `capacityValueField` inputs to optionally
factor capacity into assignment. Each user in a queue has an available capacity — the amount
of additional work they can accept. When a capacity value is specified on the item, only
users whose available capacity is greater than or equal to that value are eligible for
assignment. If neither input is set, capacity is not factored in and any available user
in the queue is eligible.

---

## Assign Multiple Items

**`actionName`:** `GradientWorks__AssignMultiItemAction`

Assigns a list of items to users associated with a Gradient Works Queue. The
items may be of any type (e.g. Account, Lead, etc). The assignments are
performed immediately.

You can access the results of the assignment attempts in the List of Assignment
objects included in the output. It will include information about if each assignment
completed successfully or if there was an error as well as information about the
user that received the item. You can loop over these as inputs to other
notification actions such as Send Single Assignment Email or Send Slack Message.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queue` | `String` | Yes | The Gradient Works Queue to add to. May be an Id or the queue name. |
| `assignmentDetails` | `String` | No | Optional notes to record with this assignment. |
| `capacityValue` | `Long` | No | Capacity value to apply to all items. |
| `capacityValueField` | `String` | No | Retrieve capacity value from this field on each item. |
| `items` | `List<SObject>` | No | List of items to add to the queue. May be any Salesforce object. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignments` | `List<Assignment>` | Information about each of the assignments in the same order as the input items. |
| `count` | `Integer` | The total number of assignments performed. |

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

## Assign Pending Items for Multiple Queues

**`actionName`:** `GradientWorks__AssignPendingMultiQueueAction`

Performs immediate assignment of any pending items in the specified list of
Queues. The results of each assignment are available in the List of Assignment
objects provided in the output.

Use this after adding items via Enqueue Single Item or Enqueue Multiple Items
when you need assignments to happen immediately within the flow, or when the
queues are not configured to process assignments asynchronously.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queues` | `List<String>` | No | Gradient Works Queues to assign pending items for. May be Ids or queue names. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignments` | `List<Assignment>` | All assignments performed for pending items. |
| `count` | `Integer` | The total number of assignments performed. |

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

## Assign Pending Items for Single Queue

**`actionName`:** `GradientWorks__AssignPendingSingleQueueAction`

Performs immediate assignment of any pending items in the specified Queue.
The results of each assignment are available in the List of Assignment
objects provided in the output.

Use this after adding items via Enqueue Single Item or Enqueue Multiple Items
when you need assignments to happen immediately within the flow, or when the
queue is not configured to process assignments asynchronously.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queue` | `String` | Yes | Gradient Works Queue to assign pending items for. May be an Id or name. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignments` | `List<Assignment>` | All assignments performed for pending items. |
| `count` | `Integer` | The total number of assignments performed. |

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

## Assign Single Item

**`actionName`:** `GradientWorks__AssignSingleItemAction`

Assigns an item to a user associated with a Gradient Works Queue. The item
may be of any type (e.g. Account, Lead, etc). The assignment is performed
immediately.

You can access the results of the assignment attempt in the Assignment object
included in the output. It will include information about if the assignment
completed successfully or if there was an error as well as information about the
user that received the item. You can use this Assignment object as an input
to notification actions such as Send Single Assignment Email or Send Slack Message.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `item` | `SObject` | Yes | The Salesforce object to assign. |
| `queue` | `String` | Yes | The target Gradient Works Queue. May be an Id or the queue name. |
| `assignmentDetails` | `String` | No | Optional notes to record with this assignment. |
| `capacityValue` | `Long` | No | Item capacity value. |
| `capacityValueField` | `String` | No | Retrieve capacity value from this field on the item. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignment` | `Assignment` | Information about the assignment, including who the item was assigned to. |

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

## Enqueue Multiple Items

**`actionName`:** `GradientWorks__EnqueueAction`

Adds items to a Gradient Works Queue. The items may be of any type
(e.g. Account, Lead, etc). This **does not** assign the items immediately,
instead they are put in a pending state in the Queue.

Why use this instead of Assign Multiple Items? In some cases it can be
more efficient to add many items to the queue through various steps in a
Flow and then perform the assignments all at once. You can explicitly trigger
assignment for a particular queue by including Assign Pending Items for Single Queue
or Assign Pending Items for Multiple Queues in your Flow. If you don't explicitly
trigger assignment in your Flow, Gradient Works will assign the pending
items at some point in the near future if the queue is configured to process
assignments asynchronously (the default).

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queue` | `String` | Yes | The Gradient Works Queue to add to. May be an Id or the queue name. |
| `assignmentDetails` | `String` | No | Optional notes to record with this assignment. |
| `capacityValue` | `Long` | No | Capacity value to apply to all items. |
| `capacityValueField` | `String` | No | Retrieve capacity value from this field on each item. |
| `enablePostAssignmentAutomation` | `Boolean` | No | Whether to run post-assignment automation configured on the queue for this item. Defaults to false. |
| `objects` | `List<SObject>` | No | List of items to add to the queue. |

### Outputs

_None._

---

## Enqueue Single Account to Target Book Queue

**`actionName`:** `GradientWorks__EnqueueTargetBookAccountAction`

Adds an Account to a Gradient Works Target Book Queue. The account is put in a
pending state and assigned asynchronously — Target Book Queues always process
assignments asynchronously, so no explicit Assign Pending Items step is needed.

Unlike Enqueue Single Item, this action is Account-specific and supports Target Book
capacity rules, fallback assignment policies, and post-assignment automation.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `account` | `Account` | Yes | The Account to assign. |
| `queue` | `String` | Yes | The Target Book Queue to add to. May be an Id or the Queue name. |
| `assignmentDetails` | `String` | No | Optional notes to record with this assignment. |
| `assignmentFallback` | `String` | No | Fallback policy when no rep has available capacity. Valid values: No Assignment to skip assignment, or Specific User to assign to the assignmentFallbackUser. |
| `assignmentFallbackUser` | `Id` | No | Fallback user to assign Account to when no one has available capacity. |
| `bypassCapacityEligibility` | `Boolean` | No | When true, ignores Target Book capacity when determining rep eligibility for this Account. |
| `enablePostAssignmentAutomation` | `Boolean` | No | Whether to run post-assignment automation configured on the queue for this item. Defaults to false. |

### Outputs

_None._

---

## Enqueue Single Item

**`actionName`:** `GradientWorks__EnqueueOneAction`

Adds an item to a Gradient Works Queue. The item may be of any type
(e.g. Account, Lead, etc). This **does not** assign the item immediately,
instead it is put in a pending state in the Queue.

Why use this instead of Assign Single Item? In some cases it can be
more efficient to add many items to the queue through various steps in a
Flow and then perform the assignments all at once. You can explicitly trigger
assignment for a particular queue by including Assign Pending Items for Single Queue
or Assign Pending Items for Multiple Queues in your Flow. If you don't explicitly
trigger assignment in your Flow, Gradient Works will assign the pending
items at some point in the near future if the queue is configured to process
assignments asynchronously (the default).

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `item` | `SObject` | Yes | Item to add to the queue. |
| `queue` | `String` | Yes | The Gradient Works Queue to add to. May be an Id or the queue name. |
| `assignmentDetails` | `String` | No | Optional notes to record with this assignment. |
| `capacityValue` | `Long` | No | Item capacity value. |
| `capacityValueField` | `String` | No | Retrieve capacity value from this field on the item. |
| `enablePostAssignmentAutomation` | `Boolean` | No | Whether to run post-assignment automation configured on the queue for this item. Defaults to false. |

### Outputs

_None._

---

## Schedule and Assign Single Item

**`actionName`:** `GradientWorks__AssignAndScheduleSingleItemAction`

**Note:** This action can only be used in Screen flows, one item at a time.
Scheduling must be enabled in your Gradient Works configuration.

Schedules a meeting with the user who was least recently assigned and is free
during the provided meeting time, then assigns the item to that user.
The item may be of any type (e.g. Account, Lead, etc).

By default, if no users have calendar availability at the requested time,
the action double-books the least recently assigned user. Set `ifNoneAvailable`
to `doNotAssign` to skip assignment instead of double-booking.

The assignment output contains details about the assigned user and whether the assignment
succeeded. It can be passed as input to notification actions such as
Send Single Assignment Email or Send Slack Message.

If an API error occurs during scheduling, this action will return an error
and stop attempting to schedule or assign the item. Add a Fault Path
to your Flow to perform additional steps as a result of this error.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `item` | `SObject` | Yes | The Salesforce object to assign. |
| `meetingEndTime` | `Datetime` | Yes | What time the meeting should be scheduled to end at. |
| `meetingStartTime` | `Datetime` | Yes | What time the meeting should be scheduled to start at. |
| `meetingTitle` | `String` | Yes | Title of the meeting. |
| `queue` | `String` | Yes | The target Gradient Works Queue. May be an Id or the queue name. |
| `assignmentDetails` | `String` | No | Optional notes to record with this assignment. |
| `capacityValue` | `Long` | No | Capacity value to apply to the assigned item. |
| `capacityValueField` | `String` | No | Retrieve capacity value from this field on the item. |
| `ifNoneAvailable` | `String` | No | One of "doNotAssign" or "doubleBook". The default is "doubleBook" if no value is provided. |
| `meetingDescription` | `String` | No | Description of the meeting. |
| `meetingGuestList` | `String` | No | Who should attend the meeting. |
| `meetingLocation` | `String` | No | Where the meeting is going to take place. |
| `meetingLocationField` | `String` | No | Retrieve value of meeting location from this field on User object. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignment` | `Assignment` | Information about the assignment, including who the item was assigned to. |
| `calendarEvent` | `CalendarEvent__c` | Details of the scheduled calendar event. |
| `errorMessage` | `String` | Message describing any error that occurs during scheduling. |
| `isSuccess` | `Boolean` | Whether scheduling the calendar event was successful or not. |

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
