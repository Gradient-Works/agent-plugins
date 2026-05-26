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

  You can access information about the created Tasks in the output.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `priority` | `String` | Yes | Indicates the importance of a Task. |
| `status` | `String` | Yes | Initial status for the created Task. |
| `subject` | `String` | Yes | Task Subject Line. |
| `comments` | `String` | No | Any comments about the Task. |
| `dueDate` | `Date` | No | A Due Date for the created Task. A Date or Datetime formula can be used or a date explicitly given. |
| `relatedPerson` | `SObject` | No | The Contact or Lead related to the Task. |
| `relatedPersonId` | `String` | No | The Contact or Lead Id related to the Task. |
| `relatedRecord` | `SObject` | No | The nonhuman object the Task is related to, typically an Account, Opportunity, Campaign, etc. A full list of valid object types can be found [under WhatId for Task](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_task.htm). |
| `relatedRecordId` | `String` | No | The nonhuman record Id related to the Task, typically an Id for an Account, Opportunity, Campaign, etc. |
| `taskData` | `String` | No | Additional Task fields to update upon Task creation. |
| `taskOwner` | `SObject` | No | The User record to assign the Task to. |
| `taskOwnerId` | `Id` | No | A User or Queue Id to assign the Task to. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `task` | `Task` | The created task. |

---

## Send Slack Message

**Action class:** `SendChatNotificationAction`

<aside class="notice">
 The Gradient Works Slack app must be installed in your workspace
 to use this action. This action runs asynchronously and does not have any
 result outputs.
</aside>

Use this action to send a Slack message to a channel or as a direct message.

The message body supports @ mentioning other users in a couple of ways:

1. Using a Salesforce User record or Id (The User's email must match in Salesforce and Slack)
2. Slack display name

Need to show additional record information at a glance?

Use the "Fields to Display" section of the action editor to customize what
information we include about Salesforce records in the Slack message.

To add a link to a record in the Slack message, add a record in the
"Linked Records" section. We'll include a "View in Salesforce" button in the
Slack message.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `message` | `String` | Yes | The main message content. |
| `channel` | `String` | No | A Slack channel id or name to send a message to |
| `fieldsJson` | `String` | No | List of fields to display in Slack message |
| `linkedRecordsJson` | `String` | No | List of records to link to in Slack message |
| `notificationType` | `String` | No | The type of message that will be sent. One of: Channel or Direct Message |
| `recipientApiId` | `String` | No | A Slack User Id to send a direct message to |
| `recipientIdentifier` | `String` | No | A Salesforce User Id or Email to send a direct message to |
| `recipientRecord` | `User` | No | A Salesforce User record to send a direct message to |

### Outputs

_None._

---
