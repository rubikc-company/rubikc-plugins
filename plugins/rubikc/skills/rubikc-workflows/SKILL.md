---
name: rubikc-workflows
description: Use Rubikc MCP tools to inspect storefronts, analyze performance, and manage storefront content safely.
---

# Rubikc workflows

Use this skill for Rubikc storefront, content, asset, navigation, structure, and analytics work.

## Start

1. Call `getMcpStatus` when diagnosing the connection.
2. Call `listStorefronts` before storefront-specific work and select the correct `storefrontId`.
3. Inspect existing state before proposing or making changes.

## Safe changes

- Prefer preview, dry-run, and read operations before mutations when available.
- Do not publish content, delete records, revoke access, or perform other destructive actions unless the user explicitly requests that outcome.
- Before a broad mutation, state which storefront and resources will change.
- Use Rubikc-hosted asset URLs in content. Import or upload external assets before attaching them.

## Analytics

- Start with storefront overview and time-series tools before drilling into cubes or sessions.
- Use real segment keys and values returned by Rubikc rather than guessing filters.
- Treat small samples cautiously and distinguish main-feed visibility from detail views.
