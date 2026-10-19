# Rubikc

Rubikc storefront tools and safe operating guidance for Cursor, ChatGPT, Codex,
Claude Code, and other Agent Plugins hosts.

The plugin includes:

- The authenticated Rubikc MCP server at `https://api2.rubikc.com/mcp`
- Skills for the main jobs:
  - `rubikc`: picking a Space, the tools, and the safety rules (drafts first,
    publish and delete only when asked)
  - `rubikc-build-space`: set up a Space from your website or source material
  - `rubikc-create-content`: create cubes and campaigns in your brand voice
  - `rubikc-analyze`: explain how your Space is doing
  - `rubikc-optimize`: fix weak content and A/B test Versions

After installation, authenticate when prompted and test the connection with:

```text
Use the Rubikc tools. Call getMcpStatus, then listStorefronts. Do not modify anything.
```

Then try one of:

```text
Build my Rubikc Space from https://example.com. Keep new cubes as drafts.
How is my Space doing this week compared to last week?
Find my five weakest cubes and draft improvements. Do not publish.
```
