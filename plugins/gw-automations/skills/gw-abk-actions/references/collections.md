# Collections

Utility actions for building and querying data structures: record maps, SOQL queries, and text collections.

## Build Record Map from Field

Takes a collection of records as `input` and the name of a field and builds
a RecordMap where the keys are the values of the specified `keyField`. You
can then use Get Record from Record Map to get a record based on a key.

To use the RecordMap later in your Flow, make sure to select
`Store Output Variables` and assign it to a variable resource with
a data type of `Apex-Defined` and an apex class of `GradientWorks__RecordMap`.
When creating the resource, **do not** check "Allow multiple values (collection)".

For example, if you specify `input` as a collection of Leads and `keyField`
as `Id`, this will generate a RecordMap you can use to look up a Lead based
on its `Id`.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `input` | `List<SObject>` | Yes | A collection of input records to use for creating a RecordMap. |
| `keyField` | `String` | Yes | The field on each record containing the value to use for that record's key in the RecordMap. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `recordMap` | `RecordMap` | Contains records from the `input` collection that can be looked up using key values obtained from each item's `keyField`. |

**`RecordMap` fields:**
A RecordMap provides a fast way to look up records based on a key using
Get Record from Record Map. This allows you to create efficient "bulk" flows
that let you fetch a large number of related records in one query that you
can access in a loop without requiring multiple Get Records actions.

To create a RecordMap, see Build Record Map from Field and
Build Record Map from Lookup.

| Field | Type | Description |
|-------|------|-------------|
| `size` | `Integer` | The number of records contained in the record map. |

---

## Build Record Map from Lookup

Takes a collection of records as `input` and the name of a lookup field
specified as `lookupField`. This will then query for all the related records
of the object specified by the `lookupField`. The fields loaded for the
related records are defined by `selectFields`. This provides an efficient
way to query for a large number of related records at once.

To use the RecordMap later in your Flow, make sure to select
`Store Output Variables` and assign it to a variable resource with
a data type of `Apex-Defined` and an apex class of `GradientWorks__RecordMap`.
When creating the resource, **do not** check "Allow multiple values (collection)".

For example, consider the following Flow structure:

1. Use `Get Records` to retrieve a collection of Contacts
2. Use a `Loop` over the collection of Contacts
3. Use `Get Records` inside the loop to retrieve the Account related to each Contact using `Contact.AccountId`

This works, but performs an additional SOQL query for each Contact. If you
have 50 Contacts, your Flow will perform 51 queries. One to get the original
Contact list and then one per Contact. You will rapidly hit Salesforce governor
limits and your Flow may run slowly.

This action allows for a pattern like so:

1. Use `Get Records` to retrieve a collection of Contacts
2. Use Build Record Map from Lookup with the Contacts as `input` to get a RecordMap of related Accounts using `AccountId` as the `lookupField` and store the RecordMap to an output variable.
3. Use a `Loop` over the collection of Contacts
4. Use Get Record from Record Map inside the loop with `Contact.AccountId` as the key to get the related Account

While this requires an extra step, it only uses 2 SOQL queries: one to get
the list of contacts and one to retrieve all the related accounts. Your flow
will be much less likely to hit governor limits and will run more quickly.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `input` | `List<SObject>` | Yes | A collection of input records with lookup fields. |
| `lookupField` | `String` | Yes | The lookup field on each input record containing the reference to the record you want to put in the RecordMap. e.g. `AccountId` or `OwnerId`. |
| `selectFields` | `String` | Yes | A comma-delimited list of fields on the related object you want to include. e.g. `Id, Name, CustomField__c`. You must specify at least one field. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `recordMap` | `RecordMap` | Contains records of the type referenced by the lookup field. The keys are the Ids of the related records. e.g. If your `lookupField` is a lookup for `Account`, the records in the RecordMap will be `Account` records and the key will be the `Id` of each `Account`. |

**`RecordMap` fields:**
A RecordMap provides a fast way to look up records based on a key using
Get Record from Record Map. This allows you to create efficient "bulk" flows
that let you fetch a large number of related records in one query that you
can access in a loop without requiring multiple Get Records actions.

To create a RecordMap, see Build Record Map from Field and
Build Record Map from Lookup.

| Field | Type | Description |
|-------|------|-------------|
| `size` | `Integer` | The number of records contained in the record map. |

---

## Build Text Collection from Field

Takes a collection of records as `input` and the name of a field and builds
a text collection containing the values of the specified `field`.

If you specify `unique` as `true`, the collection will only contain unique
values, removing duplicates.

For example, if you specify `input` as a collection of Leads and `field`
as `Email`, this will generate a collection containing a list of email
addresses. Depending on the value for `unique`, duplicate email addresses will
be removed.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `field` | `String` | Yes | The name of the field to use to get the value for the collection. All values will be turned into text, regardless of the field's data type. |
| `input` | `List<SObject>` | Yes | The collection of records to get values from. |
| `unique` | `Boolean` | No | If true, remove duplicates. Defaults to false. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `collection` | `List<String>` | The Text collection containing the values retrieved from the `field` on each `input` record (optionally with duplicates removed). |

---

## Execute SOQL

**Note:** This is an advanced action. If your query is not specific enough,
your flow may run slowly or fail. Only use this if you have a thorough
understanding of SOQL and the built-in Get Records action is too restrictive
to do what you need.

Takes a [SOQL query](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm)
and executes it within a Flow, retrieving the records according to the
specified query. This provides a useful tool for getting around some of the
limitations of the built-in `Get Records` action.

For example, consider the requirement to fetch a list of Users based on
whether their Role name contains the word "Sales". Using `Get Records` will
require you to use OR conditions with hard-coded Role Ids. Using this
action, you can use SOQL's built-in [relationship queries](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_relationships.htm) like so:

```
SELECT Id, Name, Email FROM User WHERE UserRole.Name LIKE '%Sales%'
```

Other use cases for this include IN/NOT IN queries or LIMIT clauses to
retrieve only a subset of records.

To use the results, you'll need to store the output in a variable of type
Record with the Object Type set to the object you're querying (e.g. User in
the example above). Be sure to specify that the variable is a Collection.

When you use records retrieved by this action, only the fields you specify
will be available.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `query` | `String` | Yes | The SOQL query to be executed. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `records` | `List<SObject>` | The collection of records retrieved by the SOQL query. The object type of these records is determined by the FROM clause in the SOQL query itself. |

---

## Get Record from Record Map

Retrieve a particular record from a RecordMap using the specified key. This
will commonly be used inside a `Loop` to efficiently retrieve a related record.

For example, consider the following Flow structure:

1. Use `Get Records` to retrieve a collection of Contacts
2. Use Build Record Map from Lookup to get a RecordMap of related Accounts using their `Id` as the key
3. Use a `Loop` over the collection of Contacts
4. Use Get Record from Record Map inside the loop with `Contact.AccountId` as the key to get the related Account

This approach ensures that your Flow will run efficiently within the
[Salesforce Flow limits](https://help.salesforce.com/articleView?id=sf.flow_considerations.htm&type=5) by reducing queries and CPU time.

### Inputs

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `key` | `String` | Yes | The key (usually an Id) to use for looking up a particular record. |
| `recordMap` | `RecordMap` | Yes | The RecordMap from a previous action to use for retrieval. |

### Outputs

| Field | Type | Description |
|-------|------|-------------|
| `record` | `SObject` | The record retrieved based on the key. This may be null if there is no record assigned to the specified key. |

---
