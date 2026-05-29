# Next Steps

Post-assignment actions: creating tasks, adding people to campaigns, enrolling prospects in cadences, checking cadence enrollment, and sending Slack messages.

## Add Person to Campaign

**Action class:** `GradientWorks__AddProspectToCampaignAction`

Use this action to assign a person (Lead or Contact) to an existing
Campaign. If the person already exists in the Campaign, the action returns
the existing Campaign Member.

To set additional fields on the Campaign Member, use `campaignMemberData`.
For existing Campaign Members, `campaignMemberData` fields are applied
**only** if those fields are currently blank.

`campaignMemberData` format — a JSON object where each key is a CampaignMember
field API name:

```
{"fieldApiName": {"value": "literalValue or {!flowVariable}", "dataType": "<type>"}, ...}
```

Supported `dataType` values: `String`, `Integer`, `Number`, `Boolean`, `Date`, `Datetime`, `Currency`, `Id`.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `campaignId` | `Id` | No | The Id of the Campaign to add the person to. Provide this or `campaignRecord` or `campaignName`. |
| `campaignMemberData` | `String` | No | Additional CampaignMember fields to set. For existing Campaign Members, only applies to fields that are currently blank. See the action description for the format. |
| `campaignName` | `String` | No | The Name of the Campaign to add the person to. Provide this or `campaignRecord` or `campaignId`. |
| `campaignRecord` | `Campaign` | No | The Campaign to add the person to. Provide this or `campaignId` or `campaignName`. |
| `prospectId` | `Id` | No | The Id of the Lead or Contact to assign to the Campaign. Provide this or `prospectRecord`. |
| `prospectRecord` | `SObject` | No | The Lead or Contact to assign to the Campaign. Provide this or `prospectId`. |
| `status` | `String` | No | The status of the Campaign Member when added to the Campaign. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `campaignMember` | `CampaignMember` | The created or existing CampaignMember record. |

---

## Add Person to Sales Engagement Cadence

**Action class:** `GradientWorks__AddProspectToCadenceAction`

**Note:** Authentication with a Sales Engagement Platform is required to use this action.
This action runs asynchronously and has no outputs.

Use this action to add a Lead or Contact to an existing Sales Engagement Cadence
and assign them to a specific User in the Sales Engagement platform. If the
Lead or Contact already exists in the Sales Engagement Cadence, the action
skips that Lead or Contact.

The Lead or Contact must have an Email address.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `assignToId` | `Id` | No | The Id of the User to assign the cadence member to. Provide this or `assignToRecord`. |
| `assignToRecord` | `User` | No | The User to assign the cadence member to. Provide this or `assignToId`. |
| `cadenceId` | `String` | No | The Id or Name of the Sales Engagement Cadence. |
| `prospectId` | `Id` | No | The Id of the Lead or Contact to add. Provide this or `prospectRecord`. |
| `prospectRecord` | `SObject` | No | The Lead or Contact to add. Provide this or `prospectId`. |

### Outputs

_None._

---

## Check Person Enrollment in Cadence

**Action class:** `GradientWorks__ProspectEnrollmentInCadenceAction`

Use this action to check whether a Lead or Contact is already enrolled in
a Sales Engagement Cadence.

The `status` output returns one of three values:

 * Enrolled - the person is enrolled in the cadence
 * Not Enrolled - the person is not enrolled in the cadence
 * Error - an error occurred and the enrollment status could not be determined

If an error occurs, the error details are available in the `error` output.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `cadenceId` | `String` | No | The Id or Name of the Sales Engagement Cadence. |
| `prospectEmail` | `String` | No | The email address or Id of the Lead or Contact to check enrollment for. Provide this or `prospectRecord`. |
| `prospectRecord` | `SObject` | No | The Lead or Contact to check enrollment for. Provide this or `prospectEmail`. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `error` | `String` | The error message if `status` is `Error`. Null otherwise. |
| `status` | `String` | The enrollment status: `Enrolled`, `Not Enrolled`, or `Error`. |

---

## Create Task

**Action class:** `GradientWorks__CreateTaskAction`

Use this action to immediately create and assign Tasks to users.

Use `relatedRecord` for the non-person record the Task is related to, such as
an Account or Opportunity. Use `relatedPerson` for the Contact or Lead. Both
can be specified together.

`taskData` format — a JSON object where each key is a Task field API name:

```
{"fieldApiName": {"value": "literalValue or {!flowVariable}", "dataType": "<type>"}, ...}
```

Supported `dataType` values: `String`, `Integer`, `Number`, `Boolean`, `Date`, `Datetime`, `Currency`, `Id`.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `priority` | `String` | Yes | Indicates the importance of a Task. |
| `status` | `String` | Yes | Initial status for the created Task. |
| `subject` | `String` | Yes | Task Subject Line. |
| `comments` | `String` | No | Any comments about the Task. |
| `dueDate` | `Date` | No | A Due Date for the created Task. A Date or Datetime formula can be used or a date explicitly given. |
| `relatedPerson` | `SObject` | No | The Contact or Lead to relate the Task to. Provide this or `relatedPersonId`. |
| `relatedPersonId` | `String` | No | The Id of the Contact or Lead to relate the Task to. Provide this or `relatedPerson`. |
| `relatedRecord` | `SObject` | No | The non-person record to relate the Task to, such as an Account, Opportunity, or Campaign. Do not use for Contact or Lead — use `relatedPerson` instead. Provide this or `relatedRecordId`. |
| `relatedRecordId` | `String` | No | The Id of the non-person record to relate the Task to. Provide this or `relatedRecord`. |
| `taskData` | `String` | No | Additional Task fields to update upon Task creation. |
| `taskOwner` | `SObject` | No | The User or Queue record to assign the Task to. Provide this or `taskOwnerId`. |
| `taskOwnerId` | `Id` | No | The Id of the User or Queue to assign the Task to. Provide this or `taskOwner`. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `task` | `Task` | The created task. |

---

## Send Slack Message

**Action class:** `GradientWorks__SendChatNotificationAction`

**Note:** The Gradient Works Slack app must be installed in your workspace to use
this action. This action runs asynchronously and has no outputs.

Use this action to send a Slack message to a channel or as a direct message.
Set `notificationType` to `channel` and provide `channel`, or set it to
`direct_message` and provide one of `recipientRecord`, `recipientIdentifier`,
or `recipientApiId`.

To @ mention a Salesforce User in the message body, prefix their Salesforce
User Id with `@` (e.g. `@005...`). The User's email must match in both
Salesforce and Slack.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `message` | `String` | Yes | The main message body. |
| `channel` | `String` | No | A Slack channel id or name. Used when `notificationType` is `channel`. |
| `fieldsJson` | `String` | No | A JSON array of fields to display in the Slack message. Format: `[{"label": "<display name>", "value": "<text>", "recordId": "<optional Salesforce Id>"}, ...]` |
| `linkedRecordsJson` | `String` | No | A JSON array of Salesforce records to link to in the Slack message. Format: `[{"recordId": "<Salesforce Id>"}, ...]` |
| `notificationType` | `String` | No | One of `channel` to send to a Slack channel, or `direct_message` to send as a DM. |
| `recipientApiId` | `String` | No | A Slack User Id for a direct message. Used when `notificationType` is `direct_message`. |
| `recipientIdentifier` | `String` | No | A Salesforce User Id or email address for a direct message. Used when `notificationType` is `direct_message`. |
| `recipientRecord` | `User` | No | A Salesforce User record for a direct message. Used when `notificationType` is `direct_message`. |

### Outputs

_None._

---
