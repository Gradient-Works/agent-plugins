# Reading results

`get_carve_project_scenario_account_sheet` returns a short-lived presigned URL, **not an answer**. To answer a question about the results, download the CSV to `/tmp` and analyze it:

```bash
curl -sSL "$download_url" -o /tmp/<project_id>-<scenario_id>.csv
```

Then use pandas or shell tools to filter and aggregate.

Once a carve finishes, offer to save the results for the user and ask where they want the file. Don't hand them the URL itself.

## Explaining how a carve was decided

When the user asks why an account was assigned a certain way, call `get_scenario_artifacts`. It returns the Python script the agent generated for the run. Read it to understand which inputs drove which decisions, what rules and thresholds applied, and how records were matched. Then explain it **in business terms**. Never show the script.

## Overrides: adjusting results without re-running

If the user disagrees with a handful of rows, or wants a broad rule applied on top, override rather than re-running the whole carve.

`set_carve_project_scenario_account_sheet_overrides`:
- `row_key` is the `gw_row_number` from the account sheet CSV, as an integer.
- Max 100 overrides per call, so split larger updates.
- Omit `rationale` and it is generated for you.
- `result=null` clears an override. An empty string is rejected, so use null.
- Atomic: one unknown `row_key` rejects the entire request, so nothing is partially written.
