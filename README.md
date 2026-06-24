# Rubikc AI plugins

Rubikc tools and workflows for Codex and Claude Code. Both integrations use the
authenticated MCP endpoint at `https://api2.rubikc.com/mcp`.

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
