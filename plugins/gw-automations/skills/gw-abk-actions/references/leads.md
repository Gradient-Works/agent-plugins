# Leads

Actions for converting leads.

## Convert Lead

**Action class:** `GradientWorks__ConvertLeadAction`

Converts a Lead into an Account, Contact, and optionally an Opportunity.
Pass an existing Account, Contact, or Opportunity to merge the lead into those records;
any that are omitted will be created automatically.
Use convertImmediately=false to stage the conversion without executing it — the result's
convertRequest can then be passed to Convert Multiple Leads for bulk conversion.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `convertedStatus` | `String` | Yes | Lead Status value that marks the lead as converted (where IsConverted=true). |
| `lead` | `Lead` | Yes | The Lead to convert. |
| `account` | `Account` | No | Account to associate the converted lead with. If not provided, a new Account is created from the lead's Company field. |
| `contact` | `Contact` | No | Contact to merge the lead into. If not provided, a new Contact is created from the lead's name and email. |
| `convertImmediately` | `Boolean` | No | When false, stages the conversion without executing it — the result's convertRequest can then be passed to Convert Multiple Leads. Defaults to true. |
| `createOpportunity` | `Boolean` | No | Create an Opportunity on conversion. Ignored if opportunity is specified. Defaults to false. |
| `opportunity` | `Opportunity` | No | Opportunity to associate with the converted lead. If not provided and createOpportunity is true, a new Opportunity is created. |
| `opportunityName` | `String` | No | Name for the new Opportunity when creating one. Ignored if opportunity is specified. |
| `owner` | `User` | No | Owner for any newly created Account, Contact, or Opportunity. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `account` | `Account` | The Account the lead was converted into. If account was specified in the request, this is that same Account. Otherwise a minimal Account with Id, Name, and OwnerId. Null if conversion failed. |
| `contact` | `Contact` | The Contact the lead was converted into. If contact was specified in the request, this is that same Contact. Otherwise a minimal Contact with Id, FirstName, LastName, and OwnerId. Null if conversion failed. |
| `convertRequest` | `ConvertLeadRequest` | The staged conversion — only populated when convertImmediately is false. Pass to Convert Multiple Leads to execute in bulk. |
| `errorMessage` | `String` | Error message if success is false. |
| `lead` | `Lead` | The lead from the request. Not updated to reflect conversion state — IsConverted and Converted* fields will not be set. |
| `opportunity` | `Opportunity` | The Opportunity the lead was associated with. If opportunity was specified in the request, this is that same Opportunity. Otherwise a minimal Opportunity with Id, Name, and OwnerId, or null if no opportunity was created or conversion failed. |
| `success` | `Boolean` | True if the conversion succeeded; false if it failed. |

**`ConvertLeadRequest` fields:**
A staged lead conversion, returned by Convert Lead when convertImmediately is false.
Pass a list of these to Convert Multiple Leads to execute conversions in bulk.

| Field | Type | Description |
|-------|------|-------------|
| `lead` | `Lead` | The Lead to convert. |
| `convertedStatus` | `String` | Lead Status value that marks the lead as converted (where IsConverted=true). |
| `account` | `Account` | Account to associate the converted lead with. Null if none was specified. |
| `contact` | `Contact` | Contact to merge the lead into. Null if none was specified. |
| `opportunity` | `Opportunity` | Opportunity to associate with the converted lead. Null if none was specified. |
| `createOpportunity` | `Boolean` | Whether a new Opportunity should be created on conversion. |
| `opportunityName` | `String` | Name for the new Opportunity when creating one. |
| `owner` | `User` | Owner for any newly created Account, Contact, or Opportunity. |

---

## Convert Multiple Leads

**Action class:** `GradientWorks__MultiConvertLeadAction`

Converts multiple Leads in bulk. Accepts a list of staged ConvertLeadRequest objects —
obtain these by calling Convert Lead with convertImmediately=false and collecting the
convertRequest outputs. Returns success and failure counts plus a per-lead ConvertLeadResult.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `requests` | `List<ConvertLeadRequest>` | Yes | Staged lead conversions to execute. Obtain by calling Convert Lead with convertImmediately=false and collecting the convertRequest outputs. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `failureCount` | `Integer` | Count of failed Lead conversions. Specific errors are provided in the individual result returned in results. |
| `results` | `List<ConvertLeadResult>` | List of conversion results. |
| `successCount` | `Integer` | Count of successful Lead conversions |

**`ConvertLeadResult` fields:**
Result of a single lead conversion within Convert Multiple Leads.

| Field | Type | Description |
|-------|------|-------------|
| `lead` | `Lead` | The lead that was converted. |
| `account` | `Account` | Account the lead was converted into. Null if conversion failed. |
| `contact` | `Contact` | Contact the lead was converted into. Null if conversion failed. |
| `opportunity` | `Opportunity` | Opportunity the lead was associated with. Null if none was created or conversion failed. |
| `success` | `Boolean` | True if the conversion succeeded; false if it failed. |
| `errorMessage` | `String` | Error message if success is false. |

---
