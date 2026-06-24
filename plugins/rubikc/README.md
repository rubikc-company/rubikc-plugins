# Rubikc

Rubikc storefront tools and safe operating guidance for Cursor, Claude Code,
and Codex.

The plugin includes:

- The authenticated Rubikc MCP server at `https://api2.rubikc.com/mcp`
- A workflow skill for selecting storefronts, inspecting before editing, and
  requiring explicit intent before publishing or destructive actions

After installation, authenticate when prompted and test the connection with:

```text
Use the Rubikc tools. Call getMcpStatus, then listStorefronts. Do not modify anything.
```
