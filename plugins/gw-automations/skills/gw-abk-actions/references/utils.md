# Utils

Utility actions for miscellaneous flow operations.

## Evaluate Domain against Denylist

**`actionName`:** `GradientWorks__GWFXEvaluateDomainAction`

Extracts a domain from a text `input` and checks whether it is in the
configured domain denylist. The `result` output contains the extracted
domain and whether it is in the denylist.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `input` | `String` | Yes | The text value to extract a domain from. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `result` | `DomainEvaluationResult` | The result of evaluating the extracted domain against the denylist. |

**`DomainEvaluationResult` fields:**
The result of evaluating a domain against the configured denylist.

| Field | Type | Description |
|-------|------|-------------|
| `domain` | `String` | The domain extracted from the input text. |
| `inDenylist` | `Boolean` | True if the domain is in the denylist. |

---
