# Get Started with Gradient Works

Give this page to your local agent and ask:

> Set up Gradient Works for this client. Install the plugin, complete browser
> OAuth, and verify with the read-only `who_am_i` tool.

Everything ships as one plugin, `gradient-works`. **The plugin contains both the
skills and the MCP server configuration**, so installing it does both. There is
no separate MCP setup step. The customer signs in once, the first time a
Gradient Works tool runs.

Never request API keys, tokens, authorization codes, or other credentials. The
customer authenticates through their browser.

## Agent workflow

1. Detect the current client. Do not configure unrelated clients.
2. Install the plugin using the matching client section below.
3. Start OAuth. Pause while the customer signs in, then continue.
4. Call only the read-only `who_am_i` tool.
5. Report the user, account, and client exactly as returned.

Setup is complete only when `who_am_i` succeeds. An installed plugin, a
connected indicator, or a completed browser redirect is not sufficient.

Two surfaces need the customer to click, because they have no CLI: the
**Claude apps** (Cowork, Claude Desktop, claude.ai) and **Cursor**.

## Claude Code

```shell
claude plugin marketplace add Gradient-Works/agent-plugins
claude plugin install gradient-works@gradient-works-plugins
```

Authenticate in an interactive terminal:

```shell
claude mcp login gradient-works
```

Without a terminal this exits with `stdin isn't a terminal`. Wrap it, and tail
`login.log` while the customer signs in:

```shell
sleep 900 | script -q /dev/null claude mcp login gradient-works > login.log 2>&1
```

The redirect returns to a local listener. Never ask the customer to paste a code.

### Verify from a new session

A session started before the plugin was installed cannot see the new tools. That
is stale session state, not an OAuth failure. Verify from a new session:

```shell
echo "Call only the read-only who_am_i tool from the Gradient Works MCP server. Return the response exactly." | claude -p --allowedTools "mcp__gradient-works__who_am_i"
```

Pipe the prompt via stdin; a prompt placed after flags is not parsed.

Troubleshooting:

- Inspect with `claude mcp list` and `claude mcp get gradient-works`.
- Use **Retry** or **Reconnect** in `/mcp` after a transient disconnect.
- Use **Clear authentication** in `/mcp` when OAuth belongs to the wrong user.
- Remove duplicate `gradient-works` entries across scopes.

References: [Claude Code plugins](https://code.claude.com/docs/en/plugins), [Claude Code MCP](https://code.claude.com/docs/en/mcp).

## Claude apps: Cowork, Desktop, and claude.ai

Cowork, the Chat tab in Claude Desktop, and chat on the web share one plugin
system and one install path. Plugins require a paid plan (Pro, Max, Team, or
Enterprise).

Gradient Works is not in Anthropic's public catalog, so the repository is added
as a marketplace. The agent cannot perform this install; hand over these steps
and resume afterward.

1. **Settings → Plugins**
2. **Add → Add marketplace → Add from a repository**
3. Enter `https://github.com/Gradient-Works/agent-plugins`
4. Install **Gradient Works**
5. Sign in when the connector prompts

Opening the installed plugin shows its skills and connectors, which can be
toggled individually. **Update** on the marketplace pulls later changes.

On Team and Enterprise an administrator can require the plugin for everyone, in
which case it installs automatically and members cannot remove it.

Then call `who_am_i`.

Troubleshooting:

- **Plugin not found:** use **Add marketplace**, not the built-in plugin list.
- **Skill never triggers:** enable **Settings → Capabilities → Code execution and
  file creation**. Skills do not run without it. On Team and Enterprise an Owner
  sets the same toggle under **Organization settings → Capabilities**.
- **Tools missing:** confirm the plugin and its connector are enabled.
- **Connector will not connect:** Claude reaches remote connectors from Anthropic's
  cloud, not the local machine. `https://agents.gradient.works/mcp` is public, so a
  failure points at outbound filtering rather than the customer's network.

References: [use plugins in Claude](https://support.claude.com/en/articles/13837440-use-plugins-in-claude), [install plugins in Cowork](https://claude.com/docs/cowork/guide/plugins).

## Codex

```shell
codex plugin marketplace add Gradient-Works/agent-plugins
codex plugin add gradient-works@gradient-works-plugins
codex mcp login gradient-works
```

The login command starts browser OAuth.

### Verify from a new task

A task running before the install may not discover the new tools. Prefer a new
interactive task. To finish without returning control:

```shell
codex exec --ephemeral --approve-for-me --color never \
  "Call only the read-only who_am_i tool from the Gradient Works MCP server. Return the response exactly."
```

Troubleshooting:

- Inspect with `codex mcp list`.
- Restart the Codex app or IDE extension after changes.
- For the wrong identity: `codex mcp logout gradient-works`, then log in again.

References: [Codex plugins](https://learn.chatgpt.com/docs/plugins), [Codex MCP](https://learn.chatgpt.com/docs/extend/mcp?surface=cli).

## Cursor

An individual user opens **Settings → Plugins**, adds
`https://github.com/Gradient-Works/agent-plugins` as a marketplace, and installs
**Gradient Works**.

On Team and Enterprise an admin instead opens **Dashboard → Settings → Plugins**,
selects **Import** under **Team Marketplaces**, pastes the same URL, and marks
the plugin required or optional.

Restart Cursor, open **Customize**, enable `gradient-works`, and complete browser
OAuth. Then ask Cursor to call `who_am_i`; approve that read-only tool if
prompted.

Troubleshooting:

- Restart Cursor after installing or changing configuration.
- Toggle `gradient-works` off and on under **Customize** to reconnect.
- Inspect **Output → MCP Logs** for connection or OAuth failures.
- Confirm `who_am_i` is enabled under **Available Tools**.

References: [Cursor plugins](https://cursor.com/docs/plugins), [Cursor MCP](https://cursor.com/docs/mcp).

## Fallback: install without the plugin

Use this only when the plugin route fails. It installs the skills but not the
MCP server, so also configure the server manually.

### Claude Code, Codex, and Cursor

Clone the repository if no checkout exists:

```shell
git clone --depth 1 https://github.com/Gradient-Works/agent-plugins
repo_dir="$(pwd)/agent-plugins"
```

Copy each skill directory — including its `references/` — into the client's
personal skills root:

| Client | Skills root | Manual MCP configuration |
| --- | --- | --- |
| Claude Code | `~/.claude/skills` | `claude mcp add --transport http --scope user gradient-works https://agents.gradient.works/mcp` |
| Codex | `~/.agents/skills` | `codex mcp add gradient-works --url https://agents.gradient.works/mcp` |
| Cursor | `~/.cursor/skills` | add to `~/.cursor/mcp.json` (see below) |

```shell
skills_root="$HOME/.claude/skills"   # adjust per the table
mkdir -p "$skills_root"
while IFS= read -r skill_file; do
  skill_dir="${skill_file%/SKILL.md}"
  rsync -a --delete "$skill_dir/" "$skills_root/${skill_dir##*/}/"
done < <(find "$repo_dir/plugins" -type f -path '*/skills/*/SKILL.md' -print)
```

If a same-named skill from another source already exists, stop and report the
collision rather than overwriting it. Restart the client, or run
`/reload-plugins` in Claude Code, after copying.

The Cursor entry, merged into `~/.cursor/mcp.json` alongside unrelated servers:

```json
{
  "mcpServers": {
    "gradient-works": {
      "url": "https://agents.gradient.works/mcp"
    }
  }
}
```

Then authenticate and verify with `who_am_i` as described in the client section.

### Claude apps

Plugins require a paid plan. On Free, upload each skill under **Settings → Skills
→ Add** as a ZIP whose root entry is the skill folder, then add the server under
**Settings → Connectors → + → Add custom connector**.
