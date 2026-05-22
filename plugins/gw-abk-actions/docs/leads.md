# Leads

Actions for converting leads.

## Convert Lead

Converts a Lead using functionality similar to that provided by the
Convert Lead action in the UI and the Apex function Database.convertLead.
Users may optionally supply an account, contact and opportunity to merge
the converted lead into. If any of those are not specified they will be
created.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `convertedStatus` | `String` | Yes | The name of a lead status that represents a converted lead (i.e. where IsConverted=true) |
| `lead` | `Lead` | Yes | The Lead to be converted |
| `account` | `Account` | No | If specified, associate the converted lead with this Account |
| `contact` | `Contact` | No | If specified, merge the Lead into this Contact |
| `convertImmediately` | `Boolean` | No | Set to false to prepare the lead for conversion but not actually perform it immediately. Defaults to true. |
| `createOpportunity` | `Boolean` | No | Whether or not to create an opportunity on conversion. Defaults to false. If `opportunity` is specified, this is ignored. |
| `opportunity` | `Opportunity` | No | If specified, the Opportunity to associate with the converted Lead |
| `opportunityName` | `String` | No | If creating an opportunity, the name of the opportunity to create. If `opportunity` is specified, this is ignored. |
| `owner` | `User` | No | The user that should own any newly created Account, Contact or Opportunity after conversion. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `account` | `Account` | The Account into which the converted Lead was merged. If an Account was specified in the request, this will be the same Account instance. If no account was specified, this will contain an Account object with the Id, Name and OwnerId fields. If conversion failed this will be null. |
| `contact` | `Contact` | The Contact into which the converted Lead was merged. If a Contact was specified in the request, this will be the same Contact instance. If no account was specified, this will contain a minimal Contact object with Id, FirstName, LastName and OwnerId. If conversion failed this will be null. |
| `convertRequest` | `ConvertLeadRequest` | If convertImmediately is false, the prepared conversion is returned. |
| `errorMessage` | `String` | A message describing any error during conversion. |
| `lead` | `Lead` | The lead that was included in the corresponding request. This lead *will not* be updated to reflect the conversion state. It will not contain IsConverted=true or any of the relevant Converted* fields. Minimal versions of the converted objects for Account, Contact and Opportunity are included in this result. |
| `opportunity` | `Opportunity` | The Opportunity into which the converted Lead was merged. If an Opportunity was specified in the request, this will be the same Opportunity instance. If no opportunity was specified, this will contain a minimal Opportunity object with Id, Name and OwnerId. If conversion failed, this will be null. |
| `success` | `Boolean` | True if the conversion succeeded; false if it failed. If this is false the errorMessage will be populated. |

**`ConvertLeadRequest` fields:**

| Field | Type | Description |
|-------|------|-------------|
| `lead` | `Lead` |  |
| `convertedStatus` | `String` |  |
| `account` | `Account` |  |
| `contact` | `Contact` |  |
| `opportunity` | `Opportunity` |  |
| `createOpportunity` | `Boolean` |  |
| `opportunityName` | `String` |  |
| `owner` | `User` |  |

---

## Convert Multiple Leads

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `requests` | `List<ConvertLeadRequest>` | Yes | The Leads to be converted |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `failureCount` | `Integer` | Count of failed Lead conversions. Specific errors are provided in the individual result returned in results. |
| `results` | `List<ConvertLeadResult>` | List of conversion results. |
| `successCount` | `Integer` | Count of successful Lead conversions |

**`ConvertLeadResult` fields:**

| Field | Type | Description |
|-------|------|-------------|
| `lead` | `Lead` |  |
| `account` | `Account` |  |
| `contact` | `Contact` |  |
| `opportunity` | `Opportunity` |  |
| `success` | `Boolean` |  |
| `errorMessage` | `String` |  |

---
