# Matching

Actions for matching leads to accounts, contacts, and other leads.

## Match Account to Account

Takes a list of Accounts from earlier in the Flow, finds matching Accounts and
provides a list of AccountToAccountMatch results for use later in the Flow.
The matches can be used for [assignment](/#queues-assign-single-item).

For more information on how to use this action, see [Lead and Account Matching](#lead-and-account-matching).

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `account` | `Account` | No | The Account to match to Accounts. Must fill out account or accounts. |
| `accounts` | `List<Account>` | No | The list of Accounts to match to Accounts. Must fill out account or accounts. |
| `filterData` | `String` | No | Include matches that meet all the specified filters |
| `matchCriteriaData` | `String` | No | Criteria for finding potential matching Accounts |
| `rankerData` | `String` | No | If there are multiple matches, rank them according to the specified criteria |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `count` | `Integer` | The number of results |
| `result` | `AccountToAccountMatch` | The first result of matching Accounts to Accounts. Can be used when working with a single Account in place of results. |
| `results` | `List<AccountToAccountMatch>` | The results of matching Accounts to Accounts |

**`AccountToAccountMatch` fields:**
Provides information about Accounts that match an Account as a result of
executing MatchAccountToAccountAction.

| Field | Type | Description |
|-------|------|-------------|
| `account` | `Account` | The Account that was matched |
| `bestMatch` | `Account` | The Account (if any) that best matches the account |
| `count` | `Integer` | The number of matches. This will be 0 if there are no matches. |
| `matches` | `List<AccountMatchResult>` | All the matching accounts in order of relevance. The first item in this list will correspond to `bestMatch`. |

---

## Match Lead to Account

Takes a list of Leads from earlier in the Flow, finds matching Accounts and
provides a list of LeadToAccountMatch results for use later in the Flow.
The matches can be used for [assignment](/#queues-assign-single-item) or
[lead conversion](#leads-convert-lead).

For more information on how to use this action, see [Lead and Account Matching](#lead-and-account-matching).

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `filterData` | `String` | No | Include matches that meet all the specified filters |
| `lead` | `Lead` | No | The Lead to match to Accounts. Must fill out lead or leads. |
| `leads` | `List<Lead>` | No | The list of Leads to match to Accounts. Must fill out lead or leads. |
| `matchCriteriaData` | `String` | No | Criteria for finding potential matching Accounts |
| `rankerData` | `String` | No | If there are multiple matches, rank them according to the specified criteria |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `count` | `Integer` | The number of results |
| `result` | `LeadToAccountMatch` | The first result of matching Leads to Accounts. Can be used when working with a single Lead in place of results. |
| `results` | `List<LeadToAccountMatch>` | The results of matching Leads to Accounts |

**`LeadToAccountMatch` fields:**
Provides information about Accounts that match a Lead as a result of
executing MatchLeadToAccountAction.

| Field | Type | Description |
|-------|------|-------------|
| `lead` | `Lead` | The Lead that was matched |
| `bestMatch` | `Account` | The Account (if any) that best matches the lead |
| `count` | `Integer` | The number of matches. This will be 0 if there are no matches. |
| `matches` | `List<AccountMatchResult>` | All the matching accounts in order of relevance. The first item in this list will correspond to `bestMatch`. |

---

## Match Lead to Contact

Takes a list of Leads from earlier in the Flow, finds matching Contacts and
provides a list of LeadToContactMatch results for use later in the Flow.
The matches can be used for [assignment](/#queues-assign-single-item) or
[lead conversion](#leads-convert-lead).

For more information on how to use this action, see [Lead and Account Matching](#lead-and-account-matching).

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `filterData` | `String` | No | Include matches that meet all the specified filters |
| `lead` | `Lead` | No | The Lead to match to Contacts. Must fill out lead or leads. |
| `leads` | `List<Lead>` | No | The list of Leads to match to Contacts. Must fill out lead or leads. |
| `matchCriteriaData` | `String` | No | Criteria for finding potential matching Contacts |
| `rankerData` | `String` | No | If there are multiple matches, rank them according to the specified criteria |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `count` | `Integer` | The number of results |
| `result` | `LeadToContactMatch` | The first result of matching Leads to Contacts. Can be used when working with a single Lead in place of results. |
| `results` | `List<LeadToContactMatch>` | The results of matching Leads to Contacts |

**`LeadToContactMatch` fields:**

| Field | Type | Description |
|-------|------|-------------|
| `lead` | `Lead` |  |
| `bestMatch` | `Contact` |  |
| `count` | `Integer` |  |
| `matches` | `List<Contact>` |  |

---

## Match Lead to Lead

Takes a list of Leads from earlier in the Flow, finds matching Leads and
provides a list of LeadToLeadtMatch results for use later in the Flow.
The matches can be used for [assignment](/#queues-assign-single-item) or
[lead conversion](#leads-convert-lead).

For more information on how to use this action, see [Lead and Account Matching](#lead-and-account-matching).

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `filterData` | `String` | No | Include matches that meet all the specified filters |
| `lead` | `Lead` | No | The Lead to match to other Leads. Must fill out lead or leads. |
| `leads` | `List<Lead>` | No | The list of Leads to match to other Leads. Must fill out lead or leads. |
| `matchCriteriaData` | `String` | No | Criteria for finding potential matching Leads |
| `rankerData` | `String` | No | If there are multiple matches, rank them according to the specified criteria |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `count` | `Integer` | The number of results |
| `result` | `LeadToLeadMatch` | The first result of matching Leads to other Leads. Can be used when working with a single Lead in place of results. |
| `results` | `List<LeadToLeadMatch>` | The results of matching Leads to other Leads |

**`LeadToLeadMatch` fields:**

| Field | Type | Description |
|-------|------|-------------|
| `lead` | `Lead` |  |
| `bestMatch` | `Lead` |  |
| `count` | `Integer` |  |
| `matches` | `List<Lead>` |  |

---
