# Scenario feedback

Feedback is how the user and their team leave comments on a scenario: notes, questions, and review remarks. There are three thread types:

- **scenario**: about the scenario as a whole.
- **account**: about one account row.
- **override**: reassigns one account row to a new value and opens a thread to discuss the change.

A thread is `open` or `resolved` and holds comments oldest-first. Threads belong to the scenario, not to a particular run. Every call takes `project_id` and `scenario_id`.

## Which tool, when

| User wants to | Tool | Extra inputs |
|---|---|---|
| See what's been said, or find a thread or comment | `list_carve_scenario_feedback_threads` | `type`: `all` (default), `scenario`, `account`, `override`; `status`: `all` (default), `open`, `resolved` |
| Comment on the scenario as a whole | `create_carve_scenario_feedback_thread` | `type=scenario`, `body` |
| Comment on one account | `create_carve_scenario_feedback_thread` | `type=account`, `gw_row_number`, `body` |
| Reassign one account and discuss why | `create_carve_scenario_feedback_thread` | `type=override`, `gw_row_number`, `to_value`, `body` (optional) |
| Respond within an existing discussion | `reply_to_carve_scenario_feedback_thread` | `thread_id`, `body` |
| Fix wording in a comment | `edit_carve_scenario_feedback_comment` | `comment_id`, `body` |
| Remove a comment | `delete_carve_scenario_feedback_comment` | `comment_id` |
| Close a discussion that is settled | `resolve_carve_scenario_feedback_thread` | `thread_id` |
| Pick a closed discussion back up | `reopen_carve_scenario_feedback_thread` | `thread_id` |
| Undo an override thread's reassignment | `revert_carve_scenario_feedback_override` | `thread_id` |

`thread_id` and `comment_id` come from `list_carve_scenario_feedback_threads`, so list first when you don't already have them. If it is unclear whether the user is starting a new topic or continuing one, ask.

## Account and override threads

- `gw_row_number` comes from the account sheet CSV (see [results.md](results.md)). Look up the row the user means there; don't guess it from an account name.
- `to_value` should match a value that already appears in the scenario's results, such as a rep name or territory, spelled exactly as it is in the CSV.
- An override thread changes the row's result **as soon as it is created**. Tell the user that.
- Resolving an override thread keeps the override. Reverting puts the row back to the carve's assignment and resolves the thread. If the user says "close" or "done" and it's unclear which one they mean, ask.
- Reopening a reverted override applies it again. If the row's result has changed since the revert, the reopen is refused. Tell the user the row has changed, show its current value from the CSV, and ask if they still want the thread's value. If they do, start a new override thread on that row.

Override threads are a feedback feature. They are not the same as `set_carve_project_scenario_account_sheet_overrides` (see [results.md](results.md)). Use an override thread only when the user wants to leave feedback or discuss the change.

## Errors

A `FEEDBACK_REFUSED` error means the request was rejected, for example because of a blank comment, a missing `gw_row_number` or `to_value`, or someone else's comment. Pass the message on to the user instead of retrying.

Comments are posted as the user. Editing or deleting someone else's comment is refused.

Deleting a comment cannot be undone, so confirm with the user first.
