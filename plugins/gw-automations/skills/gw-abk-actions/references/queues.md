# Queues

Actions for enqueueing records and assigning items to users via Gradient Works queues.

## Assign Multiple Items

Assigns a list of item to users associated with a Gradient Works Queue. The
items may be of any type (e.g. Account, Lead, etc). The assignments are
performed immediately.

You can access the results of the assignment attempts in the List of Assignment
objects included in the output. It will include information about if each assignment
completed successfully or if there was an error as well as information about the
user that received the item. You can loop over these as inputs to other
actions such as SendSingleEmailNotificationAction.

If you specify a capacity using `capacityValue`
or `capacityValueField`, that value will be used in capacity calculations
when assigning the item to a user. For example, if you specify a
`capacityValue` of 5, users must have an `availableCapacity` greater than
or equal to 5 to be eligible for assignment. If no capacity is specified,
we assume a null capacity value for the item and do not perform any capacity
calculations during assignment.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queue` | `String` | Yes | The queue to add to. May be an Id or the queue name. |
| `assignmentDetails` | `String` | No | Specify any additional details you'd like to record about this assignment. |
| `capacityValue` | `Long` | No | Capacity value to apply to all items |
| `capacityValueField` | `String` | No | Retrieve capacity value from this field on each object |
| `items` | `List<SObject>` | No | List of items to add to the queue. May be any Salesforce object. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignments` | `List<Assignment>` | Information about each of the assignments in the same order as the input items |
| `count` | `Integer` | The total number of assignments performed |

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

## Assign Pending Items for Multiple Queues

Performs immediate assignment of any pending items in the specified list of
Queues. The results of each assignment are available in the List of Assignment
objects provided in the output.

You will usually use this action to trigger assignments after adding items
to Queues via EnqueueAction or EnqueueOneAction.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queues` | `List<String>` | No | Queues to assign pending items for. May be ids or queue names. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignments` | `List<Assignment>` | All assignments performed for pending items |
| `count` | `Integer` | The total number of assignments performed |

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

## Assign Pending Items for Single Queue

Performs immediate assignment of any pending items in the specified Queue.
The results of each assignment are available in the List of Assignment
objects provided in the output.

You will usually use this action to trigger assignments after adding items
to a Queue via EnqueueAction or EnqueueOneAction.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queue` | `String` | Yes | Queue to assign pending items for. May be an id or name. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignments` | `List<Assignment>` | All assignments performed for pending items |
| `count` | `Integer` | The total number of assignments performed |

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

## Assign Single Item

Assigns an item to a user associated with a Gradient Works Queue. The item
may be of any type (e.g. Account, Lead, etc). The assignment is performed
immediately.

You can access the results of the assignment attempt in the Assignment object
included in the output. It will include information about if the assignment
completed successfully or if there was an error as well as information about the
user that received the item. You can use this Assignment object as an input
to other actions such as SendSingleEmailNotificationAction.

If you specify a capacity using `capacityValue`
or `capacityValueField`, that value will be used in capacity calculations
when assigning the item to a user. For example, if you specify a
`capacityValue` of 5, users must have an `availableCapacity` greater than
or equal to 5 to be eligible for assignment. If no capacity is specified,
we assume a null capacity value for the item and
do not perform any capacity calculations during assignment.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `item` | `SObject` | Yes | The Salesforce object to assign |
| `queue` | `String` | Yes | The target queue. May be an Id or the queue name. |
| `assignmentDetails` | `String` | No | Specify any additional details you'd like to record about this assignment. |
| `capacityValue` | `Long` | No | Item capacity value |
| `capacityValueField` | `String` | No | Retrieve capacity value from this field on the item |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignment` | `Assignment` | Information about the assignment, including who the item was assigned to |

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

## Enqueue Multiple Items

Adds items to a Gradient Works Queue. The items may be of any type
(e.g. Account, Lead, etc). This **does not** assign the items immediately,
instead they are put in a pending state in the Queue.

If you specify a capacity using `capacityValue`
or `capacityValueField`, that value will be used in capacity calculations
when assigning the item to a user. For example, if you specify a
`capacityValue` of 5, users must have an `availableCapacity` greater than
or equal to 5 to be eligible for assignment.

Why use this instead of AssignMultiItemAction? In some cases it can be
more efficient to add many items to the queue through various steps in a
Flow and then perform the assignments all at once. You can explicitly trigger
assignment for a particular queue by including AssignPendingSingleQueueAction
or AssignPendingMultiQueueAction in your Flow. If you don't explicitly
trigger assignment in your Flow, Gradient Works will assign the pending
items at some point in the near future.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queue` | `String` | Yes | The queue to add to. May be an Id or the queue name. |
| `assignmentDetails` | `String` | No | Specify any additional details you'd like to record about this assignment. |
| `capacityValue` | `Long` | No | Capacity value to apply to all items |
| `capacityValueField` | `String` | No | Retrieve capacity value from this field on each object |
| `enablePostAssignmentAutomation` | `Boolean` | No | Should the queue Post Assignment Automation run for this item |
| `objects` | `List<SObject>` | No | List of items to add to the queue |

### Outputs

_None._

---

## Enqueue Single Account to Target Book Queue

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `account` | `Account` | Yes | The Account to assign |
| `queue` | `String` | Yes | The Target Book Queue to add to. May be an Id or the Queue name. |
| `assignmentDetails` | `String` | No | Specify any additional details you'd like to record about this assignment. |
| `assignmentFallback` | `String` | No | Fallback assignment policy for Accounts when no one has available capacity. |
| `assignmentFallbackUser` | `Id` | No | Fallback user to assign Account to when no one has available capacity. |
| `bypassCapacityEligibility` | `Boolean` | No | Should available Target Book capacity be ignored when assigning this Account. |
| `enablePostAssignmentAutomation` | `Boolean` | No | Should the queue Post Assignment Automation run for this item |

### Outputs

_None._

---

## Enqueue Single Item

Adds an item to a Gradient Works Queue. The item may be of any type
(e.g. Account, Lead, etc). This **does not** assign the item immediately,
instead it is put in a pending state in the Queue.

If you specify a capacity using `capacityValue`
or `capacityValueField`, that value will be used in capacity calculations
when assigning the item to a user. For example, if you specify a
`capacityValue` of 5, users must have an `availableCapacity` greater than
or equal to 5 to be eligible for assignment.

Why use this instead of AssignSingleItemAction? In some cases it can be
more efficient to add many items to the queue through various steps in a
Flow and then perform the assignments all at once. You can explicitly trigger
assignment for a particular queue by including AssignPendingSingleQueueAction
or AssignPendingMultiQueueAction in your Flow. If you don't explicitly
trigger assignment in your Flow, Gradient Works will assign the pending
items at some point in the near future.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `item` | `SObject` | Yes | Item to add to the queue |
| `queue` | `String` | Yes | The queue to add to. May be an Id or the queue name. |
| `assignmentDetails` | `String` | No | Specify any additional details you'd like to record about this assignment. |
| `capacityValue` | `Long` | No | Item capacity value |
| `capacityValueField` | `String` | No | Retrieve capacity value from this field on the item |
| `enablePostAssignmentAutomation` | `Boolean` | No | Should the queue Post Assignment Automation run for this item |

### Outputs

_None._

---

## Schedule and Assign Single Item

<aside class="warning">
This action can only be used as part of a Screen flow, working with one item
at a time. Scheduling must also be enabled correctly.
</aside>

Looks through the list of users associated with a Gradient Works Queue and
schedules a meeting with the user who was least recently assigned and is free
during the provided meeting time. The item will also be assigned to this user.
The item may be of any type (e.g. Account, Lead, etc). The assignment is
performed immediately.

By default, if there are no users with calendar availability at the requested
time, we will double book the user who was least recently assigned and assign
the item to that user. If you prefer that we do not double book a user
and instead do not perform any assignment, specify `doNotAssign` as the
`ifNoneAvailable` input.

If you specify a capacity using `capacityValue`
or `capacityValueField`, that value will be used in capacity calculations
when scheduling the meetings and assigning the item to a user. For example,
if you specify a `capacityValue` of 5, users must have an `availableCapacity`
greater than or equal to 5 to be eligible for assignment. If no capacity is specified,
we assume a null capacity value for the item and do not perform any capacity
calculations during assignment.

You can access the results of the assignment attempt in the [Assignment](#assignment) object
included in the output. It will include information about if the assignment
completed successfully or if there was an error as well as information about the
user that received the item. You can use this [Assignment](#assignment) object as an input
to other actions such as
[Notifications: Send Single Assignment Email](#notifications-send-single-assignment-email).

If an API error occurs during scheduling, this action will return an error
and stop attempting to schedule or assign the item. Add a Fault Path
to your Flow to perform additional steps as a result of this error.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `item` | `SObject` | Yes | The Salesforce object to assign |
| `meetingEndTime` | `Datetime` | Yes | What time the meeting should be scheduled to end at |
| `meetingStartTime` | `Datetime` | Yes | What time the meeting should be scheduled to start at |
| `meetingTitle` | `String` | Yes | Title of the meeting |
| `queue` | `String` | Yes | The target queue. May be an Id or the queue name. |
| `assignmentDetails` | `String` | No | Specify any additional details you'd like to record about this assignment. |
| `capacityValue` | `Long` | No | Capacity value to apply to the assigned item |
| `capacityValueField` | `String` | No | Retrieve capacity from a field to apply to the assigned item |
| `ifNoneAvailable` | `String` | No | One of "doNotAssign" or "doubleBook". The default is "doubleBook" if no value is provided. |
| `meetingDescription` | `String` | No | Description of the meeting |
| `meetingGuestList` | `String` | No | Who should attend the meeting |
| `meetingLocation` | `String` | No | Where the meeting is going to take place |
| `meetingLocationField` | `String` | No | Retrieve value of meeting location from this field on User object |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignment` | `Assignment` | Information about the assignment, including who the item was assigned to |
| `calendarEvent` | `CalendarEvent__c` | Details of the scheduled calendar event |
| `errorMessage` | `String` | Message describing any error that occurs during scheduling |
| `isSuccess` | `Boolean` | Whether scheduling the calendar event was successful or not |

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
