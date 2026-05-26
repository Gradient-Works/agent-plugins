# Utils

Utility actions for miscellaneous flow operations.

## Evaluate Domain against Denylist

**Action class:** `GWFXEvaluateDomainAction`

Takes a text `input` and extracts a domain (if possible) and compares that
to the domains denylist you configured. The `result` will contain a
DomainEvaluationResult that specifies the extracted domain and whether the
domain is included in the denylist or not.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `input` | `String` | Yes | Text value to extract domain from |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `result` | `DomainEvaluationResult` | Result from evaluating the extracted domain against the Denylist |

**`DomainEvaluationResult` fields:**
Holds information about whether a domain is in the Denylist or not as a result
of executing the GWFXEvaluateDomainAction

| Field | Type | Description |
|-------|------|-------------|
| `domain` | `String` | The domain extracted from an intial text input |
| `inDenylist` | `Boolean` | If the domain is in the Denylist or not |

---
