# Rubikc AI plugins

Rubikc tools and workflows for Cursor, ChatGPT, Codex, Claude Code, and other
Agent Plugins hosts. All integrations use the authenticated MCP endpoint at
`https://api2.rubikc.com/mcp`.

The portable package is `plugins/rubikc/`: root `plugin.json` (Agent Plugins
1.0.0), `mcp.json`, and `skills/`. Vendor overlays stay in `.cursor-plugin`,
`.codex-plugin`, `.claude-plugin`, and `.mcp.json`.

## Skills

Each skill is a merchant goal, not a tool area:

| Skill | Goal |
|---|---|
| `rubikc` | Core operating guide: pick a Space, vocabulary, tool families, toolsets, safety rules. Routes to the others. |
| `rubikc-build-space` | Bootstrap a launch-ready Space from an existing website, Shopify import, or source material. |
| `rubikc-create-content` | New cubes, campaigns, shared blocks, surveys and translations from a brief, files, product data or images. |
| `rubikc-analyze` | Read-only: explain what is happening in a Space and recommend actions. |
| `rubikc-optimize` | Audit content and performance, fix weak cubes as drafts, and A/B test Versions with traffic splits. |

Skills describe the workflow and the judgment calls. Domain rules (block
schemas, metric definitions, Sources behaviour) live in the MCP `instructions`
tool and `inspectBlockTypes`, which ship with the API. Point to them instead of
copying them, so skills do not go stale when the MCP changes.

When the MCP tool list changes, check every tool name the skills mention:

```bash
grep -ohE '`[a-z][A-Za-z]+`' plugins/rubikc/skills/*/SKILL.md | sort -u
```

## Smoke test

After install and authentication:

```text
Use the Rubikc tools. Call getMcpStatus, then listStorefronts. Do not modify anything.
```

Tested: this Cursor smoke test has been run successfully against the live MCP.

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
the Rubikc server when prompted. The skills are available as
`/rubikc:rubikc`, `/rubikc:rubikc-build-space`, `/rubikc:rubikc-create-content`,
`/rubikc:rubikc-analyze`, and `/rubikc:rubikc-optimize`, and also load
automatically when a request matches.

For local marketplace development:

```text
/plugin marketplace add /absolute/path/to/rubikc-plugins
/plugin install rubikc@rubikc
```

## Other Agent Plugins hosts

Cursor, ChatGPT, Codex, GitHub Copilot, Kiro, and VS Code can load the portable
core from `plugins/rubikc/plugin.json` plus `mcp.json` and `skills/`. Point the
host at `plugins/rubikc` and authenticate the remote MCP server when prompted.
