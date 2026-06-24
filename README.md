# Rubikc AI plugins

Rubikc tools and workflows for Cursor, Codex, and Claude Code. All integrations
use the authenticated MCP endpoint at `https://api2.rubikc.com/mcp`.

## Cursor

After the repository is listed in the Cursor Marketplace, install Rubikc from
Cursor's Plugins settings. For private or team distribution, import this
repository as a team marketplace. Authenticate the Rubikc MCP server when
Cursor prompts you.

Submit a public release at
[cursor.com/marketplace/publish](https://cursor.com/marketplace/publish).

For local development with Cursor Agent:

```bash
agent \
  --plugin-dir /absolute/path/to/rubikc-plugins/plugins/rubikc \
  --trust \
  --approve-mcps
```

Test with:

```text
Use the Rubikc tools. Call getMcpStatus, then listStorefronts. Do not modify anything.
```

The plugin contains `mcp.json` for Cursor while sharing the same workflow skill
and assets with the Codex and Claude Code integrations.

## Claude Code

Add this repository as a marketplace and install the plugin:

```text
/plugin marketplace add rubikc-company/rubikc-plugins
/plugin install rubikc@rubikc
```

Restart Claude Code or run `/reload-plugins`, then open `/mcp` and authenticate
the Rubikc server when prompted.

Test with:

```text
Use the Rubikc tools. Call getMcpStatus, then listStorefronts. Do not modify anything.
```

The bundled workflow skill is available as `/rubikc:rubikc-workflows`.

For local marketplace development:

```text
/plugin marketplace add /absolute/path/to/rubikc-plugins
/plugin install rubikc@rubikc
```

## Codex

Add the repository marketplace, install `rubikc`, start a new thread, and
complete OAuth when prompted.
