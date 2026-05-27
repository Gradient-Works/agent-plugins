# Gradient Works Agent Plugins

This repository provides the official collection of Gradient Works agent plugins. It includes plugins for working with the Gradient Works platform, including the Automation Builder Kit (ABK), Salesforce flow configuration, and related areas.

Plugins are contributed and maintained by Gradient Works.

## 🗂️ Structure

```
agent-plugins/
├── plugins/                        # Plugins
│   └── gw-automations/             # Gradient Works automations plugin
│       ├── .claude-plugin/         # Claude Code manifest
│       ├── .codex-plugin/          # Codex manifest
│       ├── .cursor-plugin/         # Cursor manifest
│       ├── skills/
│       │   └── gw-abk-actions/     # ABK flow actions skill
│       │       ├── references/     # Reference docs by action category
│       │       └── SKILL.md
│       └── README.md
├── .agents/
│   └── plugins/
│       └── marketplace.json        # Codex marketplace catalog
├── .claude-plugin/
│   └── marketplace.json            # Claude Code marketplace catalog
├── .cursor-plugin/
│   └── marketplace.json            # Cursor marketplace catalog
├── LICENSE
└── README.md
```

## 🚀 Usage

### Claude Code

```shell
/plugin marketplace add Gradient-Works/agent-plugins
/plugin install gw-automations@gradient-works-plugins
```

### Codex

```shell
codex plugin marketplace add Gradient-Works/agent-plugins
```

Then browse and install **gw-automations** from the Plugin Directory.

### Cursor

1. Go to **Settings → Plugins**
2. Paste `https://github.com/Gradient-Works/agent-plugins` in the search bar and add the marketplace
3. Install **gw-automations** from the marketplace panel

## 🔌 Plugins

### gw-automations

Reference documentation for all Gradient Works ABK (Automation Builder Kit) invocable actions that can be configured in Salesforce flows.

**Included skills:**

- **gw-abk-actions** — Look up action reference docs by category (queues, matching, flows, next steps, collections, dynamic books, users, logs, events, leads, assign, notifications, utils). Use when building or interpreting Salesforce flow configurations that include Gradient Works actions.

### Other tools

For any other tool that supports the [Agent Skills specification](https://agentskills.io/):

```shell
npx skills add Gradient-Works/agent-plugins
```

## Project Governance & Support

- [License](./LICENSE)
