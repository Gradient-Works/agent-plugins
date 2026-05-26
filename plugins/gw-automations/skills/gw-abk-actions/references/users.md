# Users

Actions for reading and updating rep capacity and weight on a Gradient Works queue.

## Get Used Capacity

**Action class:** `GetUsedCapacityAction`

Use this action after configuring [Capacity Meters](#capacity-meter) to check
a Gradient Works Queue User's current used capacity, giving you more control
before performing assignments.

Access the user's calculated capacity in the `usedCapacity` output. You can
then use this value in Decisions in your Flow to determine whether or not
a user has available capacity to be assigned an item or not.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `capacityMeterIdentifier` | `String` | No | The Capacity Meter Id or Name that will be used to perform capacity calculation |
| `capacityMeterRecord` | `CapacityMeter__c` | No | The Capacity Meter Record that will be used to perform capacity calculation |
| `userId` | `Id` | No | The Salesforce User Id to calculate used capacity for |
| `userRecord` | `User` | No | The Salesforce User record to calculate used capacity for |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `usedCapacity` | `Double` | The used capacity of the specified User |

---

## Update Capacity

**Action class:** `UpdateQueueUserCapacityAction`

This action allows you to build flows that update the capacity values on
users associated with a Queue. This allows you to control the available
capacity for a user based on custom logic.

Capacity requires two values: a maximum and an amount used. `max - used` is
the available capacity which is used for assignment.

There are two ways of using this action. You can either specify explicit
values to use for capacity via `usedCapacityValue` and `maxCapacityValue` or
you can "join" data from fields on the User via `usedCapacityValueField` and
`maxCapacityValueField`. The "join" method will retrieve the values for the
specified field from each User and copy them over to the custom object that
associates the User with the Queue.

<aside class="notice">
The Users must already be associated with the Queue or they will be ignored.
</aside>

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queue` | `String` | Yes | The queue the users are assigned to |
| `users` | `List<User>` | Yes | List of users to update capacity values for |
| `maxCapacityValue` | `Long` | No | Max capacity value for the user for the specified queue |
| `maxCapacityValueField` | `String` | No | User field from which to retrieve max capacity value for the specified queue |
| `usedCapacityValue` | `Long` | No | Used capacity value for the user for the specified queue |
| `usedCapacityValueField` | `String` | No | User field from which to retrieve used capacity value for the specified queue |

### Outputs

_None._

---

## Update Weight

**Action class:** `UpdateQueueUserWeightAction`

This action allows you to build flows that update the weight values on
users associated with a Queue. This allows you to control the relative
number of "slots" that the user receives for round robin assignment.

There are two ways of using this action. You can either specify explicit
values to use for weight via `weightValue` or you can "join" data from a
field on the User via `weightField`. The "join" method will retrieve the
value for the specified field from each User and copy it over to the custom
object that associates the User with the Queue.

<aside class="notice">
The Users must already be associated with the Queue or they will be ignored.
</aside>

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queue` | `String` | Yes | The name or id of a Gradient Works queue |
| `users` | `List<User>` | No | Limit updates to only these users. Leave blank to update all users assigned to the queue. |
| `weight` | `Long` | No | Weight to assign to users |
| `weightField` | `String` | No | Retrieve weight value from this field on User object |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `weights` | `List<QueueUserWeight>` | Information about the weights applied to each user |

**`QueueUserWeight` fields:**
Provides information about the weight specified for a User as a result of
executing UpdateQueueUserWeightAction.

| Field | Type | Description |
|-------|------|-------------|
| `queue` | `String` | The Queue for which weights were updated. This will be either an Id or name depending on how the Queue was identified in the action. |
| `userId` | `Id` | The Id of a User that was updated |
| `weight` | `Long` | The new weight assigned to the User |

---
