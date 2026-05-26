# Dynamic Books

Actions for checking account eligibility against target books and triggering retrievals.

## Check Account Eligibility

<aside class="warning">
Bookbuilder must be enabled to use this action.
</aside>

Use this action to determine whether an Account is eligible for assignment to the
specified User based on the User's assigned Target Book.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `accountId` | `Id` | Yes |  |
| `userId` | `Id` | Yes |  |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `errorMessage` | `String` | Message describing any errors during action execution |
| `isEligibleForAssignment` | `Boolean` | Whether or not the Account is eligible for assignment based on Target Book criteria and a User's available capacity |
| `isSuccess` | `Boolean` | Whether the action completed successfully or not |
| `isTargetBookMatch` | `Boolean` | Whether or not the Account matches any Target Book Segment criteria |
| `maxTargetBookCapacity` | `Decimal` | The User's Target Book Max Distribution Amount |
| `reason` | `String` | The reason an Account is not eligible for assignment to the User |
| `usedTargetBookCapacity` | `Decimal` | The User's currently used Target Book capacity |

---

## Retrieval

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `distributionDefinitionIdentifier` | `String` | No | The Retrieval Template Id or Name from Bookbuilder to be used for this Retrieval. |
| `distributionDefinitionRecord` | `DistributionDefinition__c` | No | Retrieval Template from Bookbuilder to be used for this Retrieval. |

### Outputs

_None._

---

## Retrieval

<aside class="warning">
Bookbuilder must be enabled to use this action.
</aside>

Use this action to execute an account retrieval to pull accounts from reps books.
When used in a schedule-triggered flow, this action allows for automatically
pulling back unworked accounts on a regular schedule from a team of reps so
that the accounts can be redistributed to others.

To use this action, first set up a retrieval template in Bookbuilder to define
the retrieval you’d like to use. Then in the Flow action, select the Retrieval
Template you’ve previously configured to run in the flow.

Use the assignment or assignments output to see information about the assignments
that were performed as part of a Flow run. You can use the individual Assignment
objects as an input to other actions such as
[Notifications: Send Single Assignment Email](#notifications-send-single-assignment-email).

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `distributionDefinitionIdentifier` | `String` | No | The Retrieval Template Id or Name from Bookbuilder to be used for this Retrieval. |
| `distributionDefinitionRecord` | `DistributionDefinition__c` | No | Retrieval Template from Bookbuilder to be used for this Retrieval. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `assignments` | `List<Assignment>` | Information about each of the assignments from the Retrieval. |
| `count` | `Integer` | The total number of successful assignments performed from the Retrieval. |
| `errorMessage` | `String` | Message describing any errors in the Retrieval. |
| `isSuccess` | `Boolean` | Whether the Retrieval was successful or not. |

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
