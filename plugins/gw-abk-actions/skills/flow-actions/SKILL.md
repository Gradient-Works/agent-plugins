---
name: flow-actions
description: Look up Gradient Works ABK (Automation Builder Kit) invocable action reference docs. Use this when interpreting a flow configuration returned by get_flow_version, or when building a new Salesforce flow and you need to know which Gradient Works actions to use and what inputs/outputs they expect.
argument-hint: "<category or action name, e.g. 'matching', 'queues', or 'Convert Lead'>"
allowed-tools: WebFetch
---

# Gradient Works ABK Flow Actions

Reference docs for all Gradient Works invocable actions, organized by category.

## Categories

| Category | Slug | Description |
|----------|------|-------------|
| Queues | `queues` | Enqueue records and assign items to users |
| Matching | `matching` | Match leads to accounts, contacts, and other leads |
| Flows | `flows` | Lifecycle actions — Start, Finish, Resume, Failed, Execute Subflow |
| Next Steps | `next-steps` | Post-assignment actions: tasks, cadences, Slack, email |
| Collections | `collections` | Build record maps, run SOQL queries, build text collections |
| Dynamic Books | `dynamic-books` | Check account eligibility against target books, trigger retrievals |
| Users | `users` | Read and update rep capacity and weight on a queue |
| Logs | `logs` | Write messages to the Gradient Works log for flow debugging |
| Events | `events` | Handle Gradient Works platform events |
| Leads | `leads` | Convert leads individually or in bulk |
| Assign | `assign` | Direct assignment actions |
| Notifications | `notifications` | Send assignment email notifications |
| Utils | `utils` | Utility actions including domain denylist evaluation |

## How to execute

### Step 1: Determine which categories are relevant

Based on the argument or context:

- If a specific category slug or action name was provided, fetch that category.
- If interpreting an existing flow config, scan the action node names/types present and identify which categories they belong to using the table above.
- If building a new flow from a description, identify which categories cover the operations described. When in doubt, fetch multiple.

### Step 2: Fetch the relevant category docs

For each relevant category, fetch:

```
https://agents.gradient.works/docs/flow-actions/<slug>.md
```

For example:
```
https://agents.gradient.works/docs/flow-actions/matching.md
https://agents.gradient.works/docs/flow-actions/queues.md
```

### Step 3: Return the information

- When **interpreting** a flow config: explain what each action node does, what its inputs mean, and what outputs it produces.
- When **building** a flow: list the relevant actions with their required and optional inputs, and show how outputs from one action can feed into inputs of the next.
