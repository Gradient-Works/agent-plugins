# Users

Actions for reading and updating rep capacity and weight on a Gradient Works queue.

## Get Used Capacity

**`actionName`:** `GradientWorks__GetUsedCapacityAction`

Use this action after configuring Capacity Meters to check a Gradient Works
Queue User's current used capacity before performing assignments.

Use the `usedCapacity` output to determine whether a user has available
capacity to receive an assignment.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `capacityMeterIdentifier` | `String` | No | The Id or Name of the Capacity Meter. Provide this or `capacityMeterRecord`. |
| `capacityMeterRecord` | `CapacityMeter__c` | No | The Capacity Meter record. Provide this or `capacityMeterIdentifier`. |
| `userId` | `Id` | No | The Id of the User to calculate used capacity for. Provide this or `userRecord`. |
| `userRecord` | `User` | No | The User record to calculate used capacity for. Provide this or `userId`. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `usedCapacity` | `Double` | The used capacity of the specified User |

---

## Update Capacity

**`actionName`:** `GradientWorks__UpdateQueueUserCapacityAction`

Use this action to update the capacity values on users associated with a Queue.

Capacity requires two values: a maximum and an amount used. `max - used` is
the available capacity which is used for assignment.

Provide explicit values via `usedCapacityValue` and `maxCapacityValue`, or
copy values from User fields via `usedCapacityValueField` and
`maxCapacityValueField`.

**Note:** The Users must already be associated with the Queue or they will
be ignored.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queue` | `String` | Yes | The Id or Name of the queue the users are assigned to. |
| `users` | `List<User>` | Yes | The users to update capacity values for. |
| `maxCapacityValue` | `Long` | No | Explicit max capacity value. Provide this or `maxCapacityValueField`. |
| `maxCapacityValueField` | `String` | No | User field API name from which to copy the max capacity value. Provide this or `maxCapacityValue`. |
| `usedCapacityValue` | `Long` | No | Explicit used capacity value. Provide this or `usedCapacityValueField`. |
| `usedCapacityValueField` | `String` | No | User field API name from which to copy the used capacity value. Provide this or `usedCapacityValue`. |

### Outputs

_None._

---

## Update Weight

**`actionName`:** `GradientWorks__UpdateQueueUserWeightAction`

Use this action to update the weight values on users associated with a Queue.
Weight controls the relative number of items a user receives in round-robin
assignment.

Provide an explicit value via `weight`, or copy a value from a User field
via `weightField`.

**Note:** The Users must already be associated with the Queue or they will
be ignored.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `queue` | `String` | Yes | The Id or Name of the Gradient Works queue. |
| `users` | `List<User>` | No | Limit updates to only these users. Leave blank to update all users assigned to the queue. |
| `weight` | `Long` | No | Explicit weight value to assign. Provide this or `weightField`. |
| `weightField` | `String` | No | User field API name from which to copy the weight value. Provide this or `weight`. |

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
