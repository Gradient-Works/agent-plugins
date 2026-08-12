# Gradient Works Agent Plugins

This repository contains the official Gradient Works plugins and skills for supported AI coding agents.

Plugins are contributed and maintained by Gradient Works.

## 🗂️ Structure

Each plugin is self-contained under `plugins/` and registered in the marketplace catalogs.

```
agent-plugins/
├── plugins/
│   └── <plugin-name>/
│       ├── .claude-plugin/plugin.json
│       ├── .codex-plugin/plugin.json
│       ├── .cursor-plugin/plugin.json
│       └── skills/
│           └── <skill-name>/
│               ├── SKILL.md
│               └── references/          # Optional supporting references
├── .agents/plugins/marketplace.json          # Codex marketplace catalog
├── .claude-plugin/marketplace.json           # Claude Code marketplace catalog
├── .cursor-plugin/marketplace.json           # Cursor marketplace catalog
├── LICENSE
└── README.md
```

## 🚀 Usage

### Claude Code

```shell
/plugin marketplace add Gradient-Works/agent-plugins
/plugin install <plugin-name>@gradient-works-plugins
```

### Codex

**From the app:**

1. Go to the **Plugins** section.
2. Open the marketplace dropdown and select **+ Add more**.
3. Add `https://github.com/Gradient-Works/agent-plugins` as the source.
4. Select **Gradient Works Plugins**, then install the desired plugins.

**From the CLI:**

```shell
codex plugin marketplace add Gradient-Works/agent-plugins
```

Then install the desired plugins from the Plugin Directory.

### Cursor

**Individual users:**

1. Go to **Settings → Plugins**.
2. Add `https://github.com/Gradient-Works/agent-plugins` as a marketplace.
3. Install the desired plugins from the marketplace panel.

**Team/Enterprise:**

1. Go to **Dashboard → Settings → Plugins → Team Marketplaces**.
2. Import `https://github.com/Gradient-Works/agent-plugins`.
3. Set the desired plugins as required or optional.

### Other tools

For tools that support the [Agent Skills specification](https://agentskills.io/):

```shell
npx skills add Gradient-Works/agent-plugins
```

## 🔌 Plugins

### gw-automations

Reference documentation for all Gradient Works ABK (Automation Builder Kit) invocable actions that can be configured in Salesforce flows.

**Included skills:**

- **gw-abk-actions** — Look up action reference docs by category (queues, matching, flows, next steps, collections, dynamic books, users, logs, events, leads, assign, notifications, utils). Use when building or interpreting Salesforce flow configurations that include Gradient Works actions.

### gw-carve

Run Carve scenarios end to end through the Gradient Works MCP tools.

**Included skills:**

- **carve** — Create projects and scenarios, send the user's instructions to the Carve agent, run a carve, build analysis cards, explain results, override rows, and deploy to CRM.

## Project Governance & Support

- [License](./LICENSE)
