---
name: carve
displayName: Gradient Works Carve
description: Run Carve scenarios end to end through the Gradient Works MCP tools. Create projects and scenarios, send the user's instructions to the Carve agent, run a carve, build analysis cards, explain results, override rows, and deploy to CRM. Use whenever the user asks to carve a book, build a territory or allocation scenario, create cards for a scenario, or understand why accounts were assigned the way they were.
---

# Carve

Carve applies allocation rules the user describes in plain language: splitting a book across reps, drawing territories, defining segments, or any similar way of dividing accounts up. You drive it through the `gradient-works-*` MCP tools; the actual work is done by the **Carve agent**, a separate agent you talk to by sending messages into a scenario.

## Vocabulary

| Term | Meaning |
|---|---|
| **Project** | The container. Owns the data and one or more scenarios. |
| **Data source** | Something attached to a project that supplies rows: an uploaded CSV or Excel file, a Salesforce report, a HubSpot list, or a market map. |
| **Account sheet** | The table scenarios read from, and where results are written back as columns. A project's primary data source *is* the account sheet, so a single source on its own is enough. Additional sources supply extra columns only once their join columns are configured, which is a web app step. |
| **Scenario** | One set of rules to apply, e.g. "split the enterprise book across these five reps by region, keep existing relationships intact." Not limited to reallocation; it also covers building new territories, segments, and similar. |
| **Carve agent** | The agent that gathers requirements and executes the scenario. |
| **Turn** | One message to the Carve agent plus its response. |
| **Carve** | One execution of a scenario against the account sheet. |
| **Card** | A visual summary of a completed scenario, shown in the Gradient Works web app. |
| **Artifacts** | The Python script the Carve agent generated for a run, i.e. the record of how it decided. |

**The user is non-technical.** They are business-oriented and do not read code. Never surface code, function names, file paths, SQL, or tool names to them.

## References

- [Setup: project and data](references/project-data.md)
- [Running a scenario](references/scenarios.md)
- [Reading results, explaining how a carve was decided, and overrides](references/results.md)
- [Cards](references/cards.md)
- [Deploying to CRM](references/deployment.md)

## Confirm before

- Retrying an attach with `use_credits=true` after a row-limit error (this spends row-overage credits)
- Running a carve
- Saving a card
- Deploying to CRM
- Downloading a file to a specific location
- Removing a data source (permanent, cannot be undone)
- Resetting a scenario's account sheet (makes that scenario's existing results inaccessible)
