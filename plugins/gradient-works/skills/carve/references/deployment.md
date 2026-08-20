# Deploying to CRM

Once results are settled, offer to push them to the user's CRM.

1. `list_crm_connections` to find an active Salesforce or HubSpot connection.
2. `get_carve_project_scenario_account_sheet_schema` for available source columns, and `list_crm_connection_metadata` for valid target fields.
3. `deploy_carve_project_to_salesforce` or `deploy_carve_project_to_hubspot` with the field mappings.
4. Poll `get_carve_project_deploy_job` until it completes; report `success_count` / `total_count`.

Deploying writes to the user's live CRM, so always confirm before triggering it.
