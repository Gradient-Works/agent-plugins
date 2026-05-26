# Dynamic Books

Actions for checking account eligibility against target books and triggering retrievals.

## Check Account Eligibility

**Note:** Bookbuilder must be enabled to use this action.

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

**Note:** Bookbuilder must be enabled to use this action.

Use this action to execute an account retrieval to pull accounts from reps' books.
When used in a schedule-triggered flow, this action allows for automatically
pulling back unworked accounts on a regular schedule from a team of reps so
that the accounts can be redistributed to others.

To use this action, first set up a retrieval template in Bookbuilder to define
the retrieval you'd like to use. Then in the Flow action, select the Retrieval
Template you've previously configured to run in the flow.

The retrieval runs asynchronously in the background. This action has no outputs —
use Bookbuilder to view retrieval results and history.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `distributionDefinitionIdentifier` | `String` | No | The Retrieval Template Id or Name from Bookbuilder to be used for this Retrieval. |
| `distributionDefinitionRecord` | `DistributionDefinition__c` | No | Retrieval Template from Bookbuilder to be used for this Retrieval. |

### Outputs

_None._

---
