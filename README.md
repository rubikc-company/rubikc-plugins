# Rubikc AI plugins

Rubikc tools and workflows for Cursor, ChatGPT, Codex, Claude Code, and other
Agent Plugins hosts. All integrations use the authenticated MCP endpoint at
`https://api2.rubikc.com/mcp`.

The portable package is `plugins/rubikc/`: root `plugin.json` (Agent Plugins
1.0.0), `mcp.json`, and `skills/rubikc-workflows`. Vendor overlays stay in
`.cursor-plugin`, `.codex-plugin`, `.claude-plugin`, and `.mcp.json`.

Keep `skills/rubikc-workflows/SKILL.md` as a thin operating layer. Domain
playbooks live in the MCP `instructions` tool; do not duplicate them in the
skill.

## Smoke test

After install and authentication:

```text
Use the Rubikc tools. Call getMcpStatus, then listStorefronts. Do not modify anything.
```

## Cursor

After the repository is listed in the Cursor Marketplace, install Rubikc from
Customize. For private or team distribution, import this repository as a team
marketplace. Authenticate the Rubikc MCP server when Cursor prompts you.

Submit a public release at
[cursor.com/marketplace/publish](https://cursor.com/marketplace/publish).

For local development, symlink the plugin and reload Cursor:

```bash
mkdir -p ~/.cursor/plugins/local
ln -s /absolute/path/to/rubikc-plugins/plugins/rubikc ~/.cursor/plugins/local/rubikc
```

Cursor Agent also accepts:

```bash
agent \
  --plugin-dir /absolute/path/to/rubikc-plugins/plugins/rubikc \
  --trust \
  --approve-mcps
```

## ChatGPT and Codex

ChatGPT Work (web and desktop) and Codex share this plugin through
`.codex-plugin/plugin.json` and `.agents/plugins/marketplace.json`.

Add the repository marketplace, install `rubikc`, start a new thread, and
complete OAuth when prompted (`authentication: ON_INSTALL`).

```bash
codex plugin marketplace add rubikc-company/rubikc-plugins
```

Local marketplace:

```bash
codex plugin marketplace add /absolute/path/to/rubikc-plugins
```

In the ChatGPT desktop app, open Work or Codex, then Plugins, and install
Rubikc from the Official Rubikc plugins source. Developer mode is only needed
when iterating on the MCP server itself, not for installing this packaged
plugin.

## Claude Code

Validate locally, then add this repository as a marketplace and install the
plugin:

```bash
claude plugin validate ./plugins/rubikc
```

```text
/plugin marketplace add rubikc-company/rubikc-plugins
/plugin install rubikc@rubikc
```

Or load the plugin directory for a single session:

```bash
claude --plugin-dir ./plugins/rubikc
```

Restart Claude Code or run `/reload-plugins`, then open `/mcp` and authenticate
the Rubikc server when prompted. The bundled workflow skill is available as
`/rubikc:rubikc-workflows`.

For local marketplace development:

```text
/plugin marketplace add /absolute/path/to/rubikc-plugins
/plugin install rubikc@rubikc
```

## Other Agent Plugins hosts

Cursor, ChatGPT, Codex, GitHub Copilot, Kiro, and VS Code can load the portable
core from `plugins/rubikc/plugin.json` plus `mcp.json` and `skills/`. Point the
host at `plugins/rubikc` and authenticate the remote MCP server when prompted.
