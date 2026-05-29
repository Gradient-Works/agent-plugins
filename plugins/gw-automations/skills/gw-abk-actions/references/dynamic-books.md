# Dynamic Books

Actions for checking account eligibility against target books and triggering retrievals.

## Check Account Eligibility

**`actionName`:** `GradientWorks__CheckTargetBookEligibilityAction`

**Note:** Bookbuilder must be enabled to use this action.

Use this action to determine whether an Account is eligible for assignment to the
specified User based on the User's assigned Target Book.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `accountId` | `Id` | Yes | The Id of the Account to check eligibility for. |
| `userId` | `Id` | Yes | The Id of the User to check eligibility for. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `errorMessage` | `String` | Error message if the action did not complete successfully. |
| `isEligibleForAssignment` | `Boolean` | True if the Account is eligible for assignment based on Target Book criteria and the User's available capacity. |
| `isSuccess` | `Boolean` | True if the action completed successfully. |
| `isTargetBookMatch` | `Boolean` | True if the Account matches at least one Target Book Segment. |
| `maxTargetBookCapacity` | `Decimal` | The User's maximum capacity as defined by their Target Book. |
| `reason` | `String` | The reason the Account is not eligible for assignment. Null when `isEligibleForAssignment` is true. |
| `usedTargetBookCapacity` | `Decimal` | The User's currently used Target Book capacity. |

---

## Retrieval

**`actionName`:** `GradientWorks__AsyncRetrievalAction`

**Note:** Bookbuilder must be enabled to use this action.

Use this action to execute an account retrieval to pull accounts from reps' books.
Specify a Retrieval Template configured in Bookbuilder using either
`distributionDefinitionIdentifier` (the template Id or Name) or
`distributionDefinitionRecord`.

The retrieval runs asynchronously in the background. This action has no outputs —
use Bookbuilder to view retrieval results and history.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `distributionDefinitionIdentifier` | `String` | No | The Id or Name of the Retrieval Template to run. Provide this or `distributionDefinitionRecord`. |
| `distributionDefinitionRecord` | `DistributionDefinition__c` | No | The Retrieval Template record to run. Provide this or `distributionDefinitionIdentifier`. |

### Outputs

_None._

---
