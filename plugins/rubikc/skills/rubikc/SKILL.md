---
name: rubikc
description: Core operating guide for the Rubikc MCP tools. Use for any Rubikc request (a Space, storefront, cubes, blocks, feed, navigation, structures, filters, languages, files, Sources, surveys, brand settings), and before the other Rubikc skills. Covers picking a Space, Admin vocabulary, which tool family does what, toolsets, payload limits, and the draft-versus-publish safety rules.
---

# Rubikc

Rubikc turns a shop into a swipeable storefront feed. A merchant's storefront is
a **Space**. A Space is built from **cubes**: one cube is roughly one page, such
as a product, collection, story or landing page. A cube is an ordered list of
**blocks**. Block 0 is the card in the feed; blocks 1+ are what a visitor sees
after opening the cube.

This skill is the operating layer: how to start, what to call, and what never to
do without asking. Detailed domain rules live in the MCP `instructions` tool, so
call it rather than guessing.

## Pick the right skill

| The user wants to… | Skill |
|---|---|
| Set up a new Space from their existing website or shop | `rubikc-build-space` |
| Make new cubes or campaigns from a brief, documents, product data or images | `rubikc-create-content` |
| Know how the Space is doing, or why something changed | `rubikc-analyze` |
| Improve weak content, fix quality issues, or run an A/B test | `rubikc-optimize` |
| Make a single change (menu, filter, language, setting, one block) | this skill |

## Start every session

1. `getMcpStatus`: confirms the login, `toolCount`, the loaded `toolsets`,
   `maxPayloadBytes` and `maxBatchItems`.
2. `listStorefronts`: choose one `storefrontId`. If there are several, ask
   which Space unless the user already named it.
3. Read before you write: `getStorefrontMap` for the shape of the Space,
   `getCubeInventory` for blocks, versions, languages and publish state.
4. Load the matching `instructions` topic before working in an area.

## Instructions topics

Call `instructions` with one of these topics. They hold the current rules and
change with the product, so read them instead of relying on memory:

- `content`: cubes, blocks, versions, shared blocks, surveys, cube metadata
- `blocks`: the block catalog and its four placements. Then call
  `inspectBlockTypes` for one type's exact schema.
- `images`: JPG 9:16 media, import and upload
- `sources`: Shopify and Bókun imports, transforms, sync, automations
- `navigation`, `structures`, `feed`, `filters`, `locales`
- `analytics`: metric definitions and which tool answers which question
- `onboarding`: launch-ready Space setup

## Speak the merchant's language

Use Admin names with the merchant. Tool names stay technical.

| Say | Tools |
|---|---|
| Space | `storefrontId`, `listStorefronts` |
| Blocks | `createCubeContent`, `updateCubeContent`, `publishCubeContent` |
| Versions | `listCubeOrders`, `insertCubeOrderItem`, `moveCubeOrderItem` |
| Traffic splits | `saveCubeVersionTest`, `startCubeVersionTest`, `compareCubeVersionTest` |
| Shared blocks | `createGlobalContent`, `updateGlobalContent` |
| Structure | navigation (`upsertNavigation`) and structures (`updateStructure`) |
| Classes, Filters | `setCubeProductClass`, `listProductFilters` |
| Sources | `listSources`, `transformSource` |
| Settings | `manageStorefrontBrand`, `getLocales`, `updateLocales` |

Never call the Admin screens "Contents", "Ordering" or "Source Materials".

## How the tools are shaped

- One tool per operation. The name says the verb: `list`, `get`, `analyze`
  (read only); `create`, `update`, `insert`, `move` (writes); `replace`,
  `delete` (destructive).
- Most writes take `dryRun: true`. Use it before any batch, delete, replace or
  transform, and show the user the result.
- Lists page with `limit` / `offset`. Follow `nextOffset` until `hasMore` is
  false before saying something is missing.
- Keep each request under `maxPayloadBytes`. Nested HTML hits the byte limit
  long before `maxBatchItems`. On HTTP 413, split the batch; do not resend the
  same payload.
- Validation errors name the field path. Fix that field. Do not retry the same
  payload.

## Toolsets

A connection loads every tool unless its MCP URL sets `?toolsets=`. Some
clients load only about 40 tools and silently drop the rest. If a tool from
these skills is missing, check `getMcpStatus` `toolsets` and tell the user which
toolset to add to the URL: `onboarding`, `cubes`, `splits`, `shared`,
`storefront`, `files`, `sources`, `catalog`, `analytics`.

## Safety rules

- **Drafting is safe; publishing is not.** Unpublished blocks and draft cubes
  are invisible to shoppers. Leave new work unpublished and new cubes as
  drafts unless the user asked to go live.
- **Publish, delete, replace, ignore, or start or finish a traffic split only
  when the user asked for that outcome.** Before a change that touches many
  cubes, say which Space and which resources will change.
- **Never invent facts.** Prices, specs, materials, availability, policies,
  reviews and discounts must come from the user, the source site, or
  `getSource` with `includeSourceData: true`.
- **Use Rubikc-hosted media only.** Import external images, video and fonts
  with `importFileFromUrl`, or upload them with `createFileUpload` and
  `completeFileUpload`. Then use the returned URL. Never put a third-party CDN
  URL in content or settings.
- **Prefer improving existing cubes over creating duplicates.** Check
  `getCubeInventory` and `listCubes` first.
- **Out of reach:** domain, checkout, Smart Cart, integrations, custom CSS and
  access. Send the merchant to Admin for those.

## Finish

End with what changed: which cubes and blocks, their IDs, what is published,
and what is still a draft. Tell the merchant to review drafts in Admin before
publishing.
