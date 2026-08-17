# Running a scenario

```
create_carve_project_scenario
        ↓
send_carve_message  ──►  get_carve_turn (every 30s)  ──►  status: completed
        ▲                                                        │
        └──────────  user revisions / answers  ◄─────────────────┘
```

**1. Create the scenario, then talk through the requirements.** Discuss what the user actually wants before sending anything. Then `send_carve_message` with the requirements.

**2. Poll `get_carve_turn` every 30 seconds.** Continue until the status is `completed`, `failed`, or `interrupted`. Do not send another message while a turn is `in_progress` (it returns 409). Each message needs a fresh UUID `idempotency_key`. If the submission outcome is unknown because of a transport failure, retry with the same key. After a confirmed failed or interrupted turn, use a new key to retry.

**3. Confirm the restated requirements.** The agent restates what it understood. Present that restatement **verbatim**, without paraphrasing or summarizing, and ask whether it looks right and whether to run. If the user has changes, send them back and repeat. If they approve, tell the agent to run.

**4. Handle open questions.** The agent may come back with decisions it cannot make alone. Put those questions to the user, get their answers, and send them back. Do not answer on the user's behalf unless they have told you to decide for them.

**5. Report the outcome.** When `status` is `completed`, `result.message` holds the agent's final reply. When `output_available` is true, results are ready, so offer to pull them via `get_carve_project_scenario_account_sheet`.

### Resuming an existing scenario

Call `get_carve_session` **first**, before sending anything. It returns current status, the requirements gathered so far, and the agent's latest message.

That is only the current state, though. If you need the earlier back-and-forth, call `get_carve_transcript` (defaults to 50 messages, capped at 200); each message carries a `turn_id` so it lines up with `get_carve_turn`. If you need prior results, pull the account sheet.
