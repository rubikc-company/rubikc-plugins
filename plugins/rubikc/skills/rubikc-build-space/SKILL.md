---
name: rubikc-build-space
description: Bootstrap a Rubikc Space into a launch-ready storefront from the merchant's existing website or Shopify shop, or from source material such as a brand deck, catalog or documents. Use when the user wants to set up, onboard, build, migrate or "make my Space look like my site", or wants their brand, playbook, cubes and menu created from what they already have.
---

# Build a Space

Goal: a Space the merchant can review and launch. That means their brand
applied, a content playbook written for them, their important pages, products
and collections as cubes with strong feed cards, and a menu that mirrors their
site. It does not mean a settings-only pass.

Follow the `rubikc` skill for startup and safety rules.

## 1. Orient

1. `getMcpStatus`, then `listStorefronts`, and pick the `storefrontId`.
2. `getOnboardingContext`: current settings, appearance, stored shop URL,
   playbook, import counts and navigation summary in one read.
3. `instructions` topic `onboarding`, then `content` and `images`.
4. Work in the Space's default language only (`getLocales`), unless the
   merchant asks for more.

## 2. Gather the input

**From a website.** Confirm the URL with the user. Read the homepage, header,
footer, top collections, top products and key information pages with your own
web tools. Collect:

- name, logo, dark logo, favicon, colors, fonts (Google Fonts URL or font files)
- tone of voice
- menu structure
- merchandising: what is featured and how products are grouped
- the 10–30 destinations that matter most, each with its evidence URL

**From source material.** Read what the user gave you: a brand book, catalog,
PDFs, product sheets, images. Pull out the same facts. Ask for what is
missing, such as a logo or the product list, instead of inventing it.

**From imported Shopify or Bókun data.** `getSourcesInboxSummary` and
`listSources` show what has been synced. Read `instructions` topic `sources`.
Turning pending Sources into cubes with `transformSource` (dry run first) is
usually better than hand-building product cubes. Product facts come from
`getSource` with `includeSourceData: true`.

If your client supports subagents, delegate the site crawl and the cube
inventory audit. Have them return summaries with URLs and IDs, and keep every
Rubikc write in the main run.

## 3. Brand

1. Import logos, favicon, fonts and key images with `importFileFromUrl`, into
   folders such as `/brand` and `/fonts`. Use only the returned Rubikc URLs.
2. `manageStorefrontBrand` with `dryRun: true`, then for real. Set the name,
   logos, `cssVariables`, fonts, general settings (description, contact,
   `sameAs`) and `contentPlaybook`:
   - `brandPrompt`: how copy should sound, with concrete do and don't rules
     taken from the site
   - `visualStylePrompt`: how cubes should look, including image style,
     framing and colors
   - `exampleCubeIds`: add these later, once good cubes exist
3. `getStorefrontContentPlaybook` to read back the resolved playbook. All
   copy you write from here on follows it.

## 4. Map destinations to cubes

1. `getStorefrontMap` and `getCubeInventory` show what already exists:
   imported cubes, drafts, structures and navigation.
2. Build a destination map, one row per destination. Each row has the source
   URL, the matching existing cube (if any), and whether to create, improve or
   skip it. Priority order: hero or brand landing, top collections, top
   products, key information pages, then the rest.
3. Show the map to the user before creating many cubes.

## 5. Build the cubes

For each target, follow the `rubikc-create-content` workflow. In short:

- Improve existing and imported cubes. Do not duplicate them.
- Block 0 is a `mainfeeditem` with Rubikc-hosted 9:16 media and a short,
  specific title.
- Add detail blocks only where they add something: a `gallery` for product
  images, concise `text` for benefits and story, and `cubepromotion` for
  real cross-links.
- Collections become structures. Fill them with `addCubeToStructure` and
  promote them with a `cubepromotion` that uses `structureId`.
- Keep new cubes as drafts until the merchant approves them.

## 6. Menu and home feed

1. `instructions` topics `navigation`, `structures` and `feed`.
2. `upsertNavigation` for the default language, mirroring the site's header,
   after the cube IDs exist. Fix structures with `updateStructure`,
   `addCubeToStructure` and `reorderStructureCubes`.
3. Pin the strongest cubes to the home feed with `setFeedPinnedCubes`.

## 7. Check before handing over

- The default-language menu exists and every link points at a real cube or
  structure.
- Several cubes are visible in the feed, and each one's block 0 is a
  media-backed `mainfeeditem`.
- No placeholder copy, no repeated preview and body text, and no third-party
  media URLs.
- Publishing: if the merchant asked for a launch-ready Space, publish only the
  cubes that are ready (`publishCubeContent`, and clear `is_draft`). Leave the
  rest as drafts and say why.

## Report

End with a report for the merchant, in these sections:

- **Brand**
- **Playbook**
- **Imported assets**
- **Cubes created**, with IDs
- **Cubes improved**, with IDs
- **Menu and structures**
- **Published, and still draft**
- **Gaps that need the merchant**

Gaps are things like missing photos, unclear facts, or pages you skipped.
