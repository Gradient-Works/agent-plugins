# Setup: project and data

Before a scenario can run, the project needs data.

1. Ask whether this goes in an existing project (`list_carve_projects`) or a new one (`create_carve_project`).
2. If the project has no data, ask the user for a file. Upload it with `upload_asset` → `complete_asset_upload`, then `attach_carve_project_data_source` with the resulting `asset://` URI.
   - **Excel:** ask which sheet before uploading. `sheet_index` is 0-based and cannot be changed after upload.
   - **Market map:** to use one as a data source, run `export_market_map_csv` first, then upload and attach that CSV.
   - **Row limit:** only the project's *first* data source is row-limit checked. Sources attached afterwards are not.
   - **Credits:** if that first attach fails with a row-limit error, the message gives the limit and the actual row count. Credits are charged on the overage only, at `ceil((rows - limit) / 10)`. Show the user that number and only retry with `use_credits=true` after they agree. Under the limit, `use_credits` costs nothing; too few credits, and the request is rejected without writing anything.

The first data source attached becomes the project's primary source, and it *is* the account sheet. A single source on its own is enough to run a scenario.

## Bringing in extra columns

Attaching a second data source does **not** merge its columns into the account sheet. The join columns have to be configured first, and there is no MCP tool that does this.

So when the user wants columns from a second file:

1. Upload and attach it as above.
2. Tell them to configure the join for that source in the Gradient Works web app, since you cannot do it from here.
3. Once they confirm, refresh the account sheet (below) so the new columns appear.

Do not attach a second source and then act as though its columns are available. They will not be.

## Refreshing the account sheet

`replace_carve_project_data_source` and `remove_carve_project_data_source` both leave the account sheet stale until it is refreshed. So does configuring a new join.

- `get_carve_project_account_sheet_refresh_preview` first: it reports which columns would be added, updated, or removed. Show that to the user.
- `refresh_carve_project_account_sheet` to apply it.
- Existing scenarios keep using the previous account sheet version until the user resets them with `reset_carve_project_scenario_account_sheet`. **Resetting makes that scenario's existing carve results inaccessible**, so always confirm before doing it.

## Changing and removing sources

- `replace_carve_project_data_source` swaps a file. The replacement must be the same type.
- `remove_carve_project_data_source` detaches a source **permanently, and this cannot be undone**. Confirm with the user first. The primary source cannot be removed at all; that request is rejected.
