# Scenario feedback

Feedback is how the user's team leaves comments on a scenario: notes, questions, and review remarks. There are three types: **scenario** (about the scenario as a whole), **account** (about one account row), and **override** (about one override). Only scenario feedback is available today, in both the web app and the MCP tools.

A thread is `open` or `resolved` and holds comments oldest-first. Every call takes `project_id` and `scenario_id`.

## Which tool, when

| User wants to | Tool | Extra inputs |
|---|---|---|
| See what's been said, or find a thread or comment | `list_carve_scenario_feedback_threads` | `status`: `all` (default), `open`, `resolved` |
| Raise a new point | `create_carve_scenario_feedback_thread` | `body` |
| Respond within an existing discussion | `reply_to_carve_scenario_feedback_thread` | `thread_id`, `body` |
| Fix wording in a comment | `edit_carve_scenario_feedback_comment` | `comment_id`, `body` |
| Remove a comment | `delete_carve_scenario_feedback_comment` | `comment_id` |
| Close a discussion that is settled | `resolve_carve_scenario_feedback_thread` | `thread_id` |
| Pick a closed discussion back up | `reopen_carve_scenario_feedback_thread` | `thread_id` |

`thread_id` and `comment_id` come from `list_carve_scenario_feedback_threads`, so list first when you don't already have them. If it is unclear whether the user is starting a new topic or continuing one, ask.

Comments are posted as the user. Editing or deleting someone else's comment is refused; pass that reason on instead of retrying.

Deleting a comment cannot be undone, so confirm with the user first.
