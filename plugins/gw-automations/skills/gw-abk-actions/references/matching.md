# Matching

Actions for matching leads to accounts, contacts, and other leads.

## Shared Input Formats

All matching actions accept three JSON string inputs for configuring match criteria, filters,
and rankers. These are normally populated by the Gradient Works custom Flow editor.

### matchCriteriaData
JSON array of match rules that determine which candidate records are considered potential matches.
Each rule: `{"subjectField":"<SubjectField>","matchType":"DOMAIN|EXACT|FUZZY","candidateField":"<CandidateField>"}`

### filterData
Applied after match criteria to narrow candidates — only candidates satisfying all conditions are included in results.
`{"conditionRequirements":"AND|OR|CUSTOM","conditionRequirementsExpression":"<expr>","conditions":[{"field":"<CandidateField>","operator":"<op>","value":"<val>"}]}`
Valid operators depend on the field type:
- All field types: EQUALS, NOT_EQUALS, IS_NULL
- Numerical (currency, percent, double, datetime, date, int, long, number, integer): + IS_IN, LESS_THAN, LESS_THAN_OR_EQUAL_TO, GREATER_THAN, GREATER_THAN_OR_EQUAL_TO
- Text and other fields: + IS_IN, CONTAINS, STARTS_WITH, ENDS_WITH
- Multi-select picklist: + INCLUDES, EXCLUDES
- Boolean, User, RecordTypeId: only EQUALS, NOT_EQUALS, IS_NULL

### rankerData
Determines which candidate is returned as bestMatch when multiple candidates are found.
Use FIELD to rank by a value on the candidate record itself (e.g. Created Date, a score field).
Use CHILD_AGGREGATE to rank by an aggregate of related child records (e.g. COUNT of Activities).
FIELD ranker: `{"kind":"FIELD","order":"HIGHER_IS_BETTER|LOWER_IS_BETTER","field":"<CandidateField>"}`
CHILD_AGGREGATE ranker: `{"kind":"CHILD_AGGREGATE","order":"HIGHER_IS_BETTER|LOWER_IS_BETTER","childRelationship":"<rel>","childField":"<field>","aggregateFunction":"AVG|COUNT|COUNT_DISTINCT|MIN|MAX|SUM"}`

---

## Match Account to Account

**Action class:** `GradientWorks__MatchAccountToAccountAction`

Flow action that matches one or more subject Accounts to candidate Accounts using
configurable match criteria, optional filters, and optional rankers.
Returns one AccountToAccountMatch per input subject Account, each containing the
best candidate found. Typically precedes an assignment step in a Flow.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `account` | `Account` | No | Subject Account to match. Provide account or accounts — not both. |
| `accounts` | `List<Account>` | No | Subject Accounts to match. Provide account or accounts — not both. |
| `filterData` | `String` | No | Filter conditions — field is an Account API field name on the candidate. See Shared Input Formats. |
| `matchCriteriaData` | `String` | No | Match rules — subjectField and candidateField are both Account API field names. See Shared Input Formats. |
| `rankerData` | `String` | No | Rankers — field references an Account API field name on the candidate. See Shared Input Formats. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `count` | `Integer` | Number of entries in results — equals the number of input subject Accounts, not the number of candidates found per Account. |
| `result` | `AccountToAccountMatch` | The first AccountToAccountMatch; convenient for single-subject flows. Null if results is empty. |
| `results` | `List<AccountToAccountMatch>` | All match results, one AccountToAccountMatch per input subject Account. |

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

**Action class:** `GradientWorks__MatchLeadToAccountAction`

Flow action that matches one or more subject Leads to candidate Accounts using
configurable match criteria, optional filters, and optional rankers.
Returns one LeadToAccountMatch per input subject Lead, each containing the
best candidate found. Typically precedes an assignment or
lead-conversion step in a Flow.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `filterData` | `String` | No | Filter conditions — field is an Account API field name on the candidate. See Shared Input Formats. |
| `lead` | `Lead` | No | Subject Lead to match. Provide lead or leads — not both. |
| `leads` | `List<Lead>` | No | Subject Leads to match. Provide lead or leads — not both. |
| `matchCriteriaData` | `String` | No | Match rules — subjectField is a Lead API field name, candidateField is an Account API field name. See Shared Input Formats. |
| `rankerData` | `String` | No | Rankers — field references an Account API field name on the candidate. See Shared Input Formats. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `count` | `Integer` | Number of entries in results — equals the number of input subject Leads, not the number of candidates found per Lead. |
| `result` | `LeadToAccountMatch` | The first LeadToAccountMatch; convenient for single-subject flows. Null if results is empty. |
| `results` | `List<LeadToAccountMatch>` | All match results, one LeadToAccountMatch per input subject Lead. |

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

**Action class:** `GradientWorks__MatchLeadToContactAction`

Flow action that matches one or more subject Leads to candidate Contacts using
configurable match criteria, optional filters, and optional rankers.
Returns one LeadToContactMatch per input subject Lead, each containing the
best candidate found. Typically precedes an assignment or
lead-conversion step in a Flow.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `filterData` | `String` | No | Filter conditions — field is a Contact API field name on the candidate. See Shared Input Formats. |
| `lead` | `Lead` | No | Subject Lead to match. Provide lead or leads — not both. |
| `leads` | `List<Lead>` | No | Subject Leads to match. Provide lead or leads — not both. |
| `matchCriteriaData` | `String` | No | Match rules — subjectField is a Lead API field name, candidateField is a Contact API field name. See Shared Input Formats. |
| `rankerData` | `String` | No | Rankers — field references a Contact API field name on the candidate. See Shared Input Formats. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `count` | `Integer` | Number of entries in results — equals the number of input subject Leads, not the number of candidates found per Lead. |
| `result` | `LeadToContactMatch` | The first LeadToContactMatch; convenient for single-subject flows. Null if results is empty. |
| `results` | `List<LeadToContactMatch>` | All match results, one LeadToContactMatch per input subject Lead. |

**`LeadToContactMatch` fields:**
Result of matching a single subject Lead to candidate Contacts. Returned as
part of MatchLeadToContactAction results — one LeadToContactMatch per input subject Lead.
The bestMatch Contact is queried for AccountId to support lead conversion.

| Field | Type | Description |
|-------|------|-------------|
| `lead` | `Lead` | The subject Lead that was matched. |
| `bestMatch` | `Contact` | The top-ranked candidate Contact, with AccountId populated. Null if no candidates were found. |
| `count` | `Integer` | Number of candidates found (equals matches.size()). |
| `matches` | `List<Contact>` | All candidate Contacts found, ordered by the configured rankers. |

---

## Match Lead to Lead

**Action class:** `GradientWorks__MatchLeadToLeadAction`

Flow action that matches one or more subject Leads to candidate Leads using
configurable match criteria, optional filters, and optional rankers.
Returns one LeadToLeadMatch per input subject Lead, each containing the
best candidate found. Typically precedes an assignment or
lead-conversion step in a Flow.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `filterData` | `String` | No | Filter conditions — field is a Lead API field name on the candidate. See Shared Input Formats. |
| `lead` | `Lead` | No | Subject Lead to match. Provide lead or leads — not both. |
| `leads` | `List<Lead>` | No | Subject Leads to match. Provide lead or leads — not both. |
| `matchCriteriaData` | `String` | No | Match rules — subjectField and candidateField are both Lead API field names. See Shared Input Formats. |
| `rankerData` | `String` | No | Rankers — field references a Lead API field name on the candidate. See Shared Input Formats. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `count` | `Integer` | Number of entries in results — equals the number of input subject Leads, not the number of candidates found per Lead. |
| `result` | `LeadToLeadMatch` | The first LeadToLeadMatch; convenient for single-subject flows. Null if results is empty. |
| `results` | `List<LeadToLeadMatch>` | All match results, one LeadToLeadMatch per input subject Lead. |

**`LeadToLeadMatch` fields:**
Result of matching a single subject Lead to candidate Leads. Returned as
part of MatchLeadToLeadAction results — one LeadToLeadMatch per input subject Lead.

| Field | Type | Description |
|-------|------|-------------|
| `lead` | `Lead` | The subject Lead that was matched. |
| `bestMatch` | `Lead` | The top-ranked candidate Lead. Null if no candidates were found. |
| `count` | `Integer` | Number of candidates found (equals matches.size()). |
| `matches` | `List<Lead>` | All candidate Leads found, ordered by the configured rankers. |

---
