# Gradient Works Agent Plugins

This repository contains the official Gradient Works plugins and skills for supported AI coding agents.

Plugins are contributed and maintained by Gradient Works.

## 🗂️ Structure

Everything ships as one plugin, `gradient-works`, registered in a marketplace
catalog per client.

```
agent-plugins/
├── plugins/
│   └── gradient-works/
│       ├── .claude-plugin/plugin.json
│       ├── .codex-plugin/plugin.json
│       ├── .cursor-plugin/plugin.json
│       ├── .mcp.json                          # MCP server for Claude Code and Codex
│       ├── mcp.json                           # MCP server for Cursor
│       └── skills/
│           └── <skill-name>/
│               ├── SKILL.md
│               └── references/                # Optional supporting references
├── .agents/plugins/marketplace.json           # Codex marketplace catalog
├── .claude-plugin/marketplace.json            # Claude Code marketplace catalog
├── .cursor-plugin/marketplace.json            # Cursor marketplace catalog
├── GETTING_STARTED.md
├── LICENSE
└── README.md
```

## 🚀 Usage

The plugin contains both the skills and the Gradient Works MCP server
configuration, so installing it does both. There is no separate MCP setup step —
you are just asked to sign in once, the first time a Gradient Works tool runs.

Claude Code, Codex, and Cursor install from the command line. Cowork, Claude
Desktop, and claude.ai share one plugin system and install through the Claude
account instead — see [Claude apps](#claude-apps-cowork-desktop-and-claudeai).

To have your own agent do all of this for you, point it at
[Get Started with Gradient Works](./GETTING_STARTED.md).

### Claude Code

```shell
/plugin marketplace add Gradient-Works/agent-plugins
/plugin install gradient-works@gradient-works-plugins
```

The MCP server comes with the plugin. Sign in when prompted, or run
`claude mcp login gradient-works`.

<details>
<summary>Configure the server manually instead</summary>

```shell
claude mcp add --transport http --scope user gradient-works https://agents.gradient.works/mcp
claude mcp login gradient-works
```

</details>

### Claude apps: Cowork, Desktop, and claude.ai

Cowork, the Chat tab in Claude Desktop, and chat on the web share one plugin
system and one install path. Plugins require a paid plan.

Gradient Works is not in Anthropic's public catalog, so add this repository as a
marketplace rather than browsing for it.

1. Go to **Settings → Plugins**.
2. Select **Add → Add marketplace → Add from a repository**.
3. Enter `https://github.com/Gradient-Works/agent-plugins`.
4. Install **Gradient Works** from the plugins it lists.
5. Sign in when the Gradient Works connector prompts for authentication.

Open the installed plugin to see its skills and connectors and toggle individual
components. Use **Update** on the marketplace to pull later changes, and
**Uninstall** on the plugin to remove it.

Skills need **Settings → Capabilities → Code execution and file creation** to be
enabled.

On Team and Enterprise, an administrator can install Gradient Works for everyone
instead; required plugins install automatically and cannot be removed by members.

<details>
<summary>Install the skills and connector manually instead</summary>

Upload each skill under **Settings → Skills → Add** as a ZIP whose root entry is
the skill folder. Then add the server under **Settings → Connectors → + → Add
custom connector**, with the name `Gradient Works` and the URL
`https://agents.gradient.works/mcp`.

On Team and Enterprise an Owner may need to enable the connector first under
**Organization settings → Connectors**.

</details>

### Codex

**From the app:**

1. Go to the **Plugins** section.
2. Open the marketplace dropdown and select **+ Add more**.
3. Add `https://github.com/Gradient-Works/agent-plugins` as the source.
4. Select **Gradient Works Plugins**, then install **Gradient Works**.

**From the CLI:**

```shell
codex plugin marketplace add Gradient-Works/agent-plugins
codex plugin add gradient-works@gradient-works-plugins
```

The MCP server comes with the plugin. Sign in when prompted, or run
`codex mcp login gradient-works`.

<details>
<summary>Configure the server manually instead</summary>

```shell
codex mcp add gradient-works --url https://agents.gradient.works/mcp
codex mcp login gradient-works
```

</details>

### Cursor

**Individual users:**

1. Go to **Settings → Plugins**.
2. Add `https://github.com/Gradient-Works/agent-plugins` as a marketplace.
3. Install **Gradient Works** from the marketplace panel.

**Team/Enterprise:**

1. An admin goes to **Dashboard → Settings → Plugins**.
2. Select **Import** under **Team Marketplaces** and paste the same URL.
3. Set **Gradient Works** as required or optional.

The MCP server comes with the plugin. Restart Cursor, enable `gradient-works`
under **Customize**, and complete browser sign-in.

<details>
<summary>Configure the server manually instead</summary>

Merge this into `~/.cursor/mcp.json`, preserving unrelated servers:

```json
{
  "mcpServers": {
    "gradient-works": {
      "url": "https://agents.gradient.works/mcp"
    }
  }
}
```

</details>

### Other tools

For tools that support the [Agent Skills specification](https://agentskills.io/):

```shell
npx skills add Gradient-Works/agent-plugins
```

This installs the skills only. Configure the MCP server
`https://agents.gradient.works/mcp` using whatever method that tool supports.

## 🔌 What's included

One plugin, `gradient-works`, containing every Gradient Works skill and the
production MCP server configuration.

**Skills:**

- **carve** — Run Carve scenarios end to end: create projects and scenarios,
  send instructions to the Carve agent, run a carve, build analysis cards,
  explain results, override rows, and deploy to CRM. Requires the MCP server.
- **gw-abk-actions** — Look up Gradient Works ABK (Automation Builder Kit)
  invocable action reference docs by category: queues, matching, flows, next
  steps, collections, dynamic books, users, logs, events, leads, assign,
  notifications, and utils. Use when building or interpreting Salesforce flow
  configurations that include Gradient Works actions. Reads local reference
  files only.

**MCP server:**

```text
https://agents.gradient.works/mcp
```

Remote HTTP with browser OAuth. Bundled with the plugin, so installing Gradient
Works configures it on every supported client.

## Project Governance & Support

- [License](./LICENSE)
