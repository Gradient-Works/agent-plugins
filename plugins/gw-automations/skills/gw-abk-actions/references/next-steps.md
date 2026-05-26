# Next Steps

Post-assignment actions: creating tasks, enrolling prospects in cadences, sending Slack or email notifications.

## Add Person to Campaign

**Action class:** `AddProspectToCampaignAction`

Use this action to assign a person (Lead or Contact) to an existing
Campaign. If the person already exists in the Campaign, the action will
return the existing Campaign Member.

You can update additional fields on the Campaign Member Record using the
"Additional Campaign Member Details" section in the editor. If an existing
Campaign Member is found, we will update any of the specified additional fields
<strong>only</strong> if those fields are blank.

Pair this with StartFlowAction to save information on newly created Campaign
Member Records about the Flow. This will help track which Campaign Members
were created as a result of this action which can be useful for reporting
and debugging.

Use the `campaignMember` output to see details about the Campaign Member
Record that was created or updated.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `campaignId` | `Id` | No | Campaign Id to add Person to |
| `campaignMemberData` | `String` | No | Additional updates to CampaignMember |
| `campaignName` | `String` | No | Campaign Name to add Person to |
| `campaignRecord` | `Campaign` | No | Campaign to add Person to |
| `prospectId` | `Id` | No | Lead or Contact Id to assign to the Campaign |
| `prospectRecord` | `SObject` | No | Lead or Contact to assign to the Campaign |
| `status` | `String` | No | Person status when added to Campaign |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `campaignMember` | `CampaignMember` | CampaignMember Record |

---

## Add Person to Sales Engagement Cadence

**Action class:** `AddProspectToCadenceAction`

<aside class="notice">
Authentication with a Sales Engagement Platform is required to use this action.
This action runs asynchronously and does not have any result outputs.
</aside>

Use this action to add a Lead or Contact to an existing Sales Engagement Cadence
and assign them to a specific User in the Sales Engagement platform. If the
Lead or Contact already exists in the Sales Engagement Cadence, the action
will skip that Lead or Contact.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `assignToId` | `Id` | No | User Id to assign the Sales Engagement Cadence member to |
| `assignToRecord` | `User` | No | User to assign the Sales Engagement Cadence member to |
| `cadenceId` | `String` | No | Sales Engagement Cadence Id or Name |
| `prospectId` | `Id` | No | Lead or Contact Id to add to Sales Engagement Cadence |
| `prospectRecord` | `SObject` | No | Lead or Contact to add to Sales Engagement Cadence |

### Outputs

_None._

---

## Check Person Enrollment in Cadence

**Action class:** `ProspectEnrollmentInCadenceAction`

Use this action to check whether or not a Person is already enrolled in
a Sales Engagement Cadence.

The `status` output will display one of three values:

 * Enrolled - the Person is enrolled in the Sales Engagement Cadence
 * Not Enrolled - the Person is not enrolled in the Sales Engagement Cadence
 * Error - an error occurred and we were not able to determine the Person status

If an error occurs, we will display the error in the `error` output

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `cadenceId` | `String` | No | Sales Engagement Cadence Id or Name |
| `prospectEmail` | `String` | No | Lead or Contact Email to check enrollment for |
| `prospectRecord` | `SObject` | No | Lead or Contact to check enrollment for |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `error` | `String` | Any errors that occur will be listed here |
| `status` | `String` | The enrollment status of the Person |

---

## Create Task

**Action class:** `CreateTaskAction`

Use this action to immediately create and assign Tasks to users.

Use `relatedRecord` for the non-person record the Task is related to, such as
an Account or Opportunity. Use `relatedPerson` for the Contact or Lead. Both
can be specified together.

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

**Action class:** `SendChatNotificationAction`

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
| `fieldsJson` | `String` | No | List of fields to display in Slack message |
| `linkedRecordsJson` | `String` | No | List of records to link to in Slack message |
| `notificationType` | `String` | No | One of `channel` to send to a Slack channel, or `direct_message` to send as a DM. |
| `recipientApiId` | `String` | No | A Slack User Id for a direct message. Used when `notificationType` is `direct_message`. |
| `recipientIdentifier` | `String` | No | A Salesforce User Id or email address for a direct message. Used when `notificationType` is `direct_message`. |
| `recipientRecord` | `User` | No | A Salesforce User record for a direct message. Used when `notificationType` is `direct_message`. |

### Outputs

_None._

---
