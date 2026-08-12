# Cards

Cards are the visual explanation of a finished scenario. Five types:

| Type | Shows |
|---|---|
| `summary` | A table of results |
| `metric` | A single headline number, formatted as `int`, `float`, `currency`, or `percent` |
| `chart` | A series rendered as `bar`, `line`, `donut`, or `bar_stacked` |
| `geo` | Results on a US map, by `state` or by `zip_code` |
| `narrative` | A written explanation in markdown |

The scenario must have been run before any card can be previewed or saved.

**Do not use `send_carve_message` for cards.** Card work goes through the card tools.

**Flow: ask → preview → confirm → save.**

1. Ask the user what they want the card to show, and which type fits. Don't guess.
2. Preview it with `preview_carve_project_scenario_card`, which opens the interactive card. Nothing is saved at this step.
3. Show the preview and ask if it looks right. Iterate if not.
4. `save_carve_project_scenario_card`, passing the preview's `config` **unchanged**. Do not hand-edit or regenerate it. Bookcarve may recompute the card's derived values when saving.

For existing cards: `list_carve_project_scenario_cards` is a lookup only (id, type, title, instructions, no data). Use it to find a card, then `view_carve_project_scenario_card` to display it, or `get_carve_project_scenario_card` when you need the raw values to compute with. When the user wants to *see* a card, open it rather than describing it from the index.

**Opening several cards: each card gets its own message block.** `view_carve_project_scenario_card` opens exactly one card, so showing the user four cards means four calls. Put **one call in each message block**: call it, let that card render, then start a new block for the next card. Never put two `view_carve_project_scenario_card` calls in the same block, because only one of them will render.

To paste a card into a doc or deck, `get_carve_project_scenario_card_image_link` mints a stable public PNG URL that always renders current data.
