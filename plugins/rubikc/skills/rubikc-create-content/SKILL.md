---
name: rubikc-create-content
description: Create or rewrite Rubikc cubes and blocks from a brief, documents, product data, web pages or images. Use when the user wants new cubes, a campaign, a landing page, product or story content, a translated version, shared blocks, surveys, or wants existing cube copy and media rewritten in their brand voice.
---

# Create content

Goal: cubes that look and read like the merchant's brand, are built from real
facts, and are ready for the merchant to review. They stay as drafts unless the
user asked to publish.

Follow the `rubikc` skill for startup and safety rules.

## 1. Understand the job

- What is it for? A product, a collection, a campaign, a story, or a landing
  page for ads. Who is it for, and what should the visitor do next?
- What is the source? A brief, files, URLs, Shopify data, or existing cubes.
- Which language or languages, and should it appear in the home feed or only
  through a direct link (`exclude_from_feed`)?

For more than three cubes, write a short plan first and confirm it. Give each
cube a title, a purpose, the facts it uses, and an outline of its blocks.

## 2. Load the rules and the brand

1. `instructions` topics `content`, `blocks` and `images`.
2. `getStorefrontContentPlaybook`. Follow its `instructions` string, then
   open each `exampleCubeIds` cube (`getCubeInfo`) to copy its rhythm, length
   and visual style.
3. `inspectBlockTypes` with `includeSchema: false` for the catalog, then once
   per block type you will use, with `contentType`, for the exact schema and
   examples.
4. Check for overlap: `listCubes` (search) or `getCubeInventory`. If a cube
   for this already exists, improve it.

## 3. Collect facts and media

- Facts come only from the user's material, the source site, or Sources
  (`getCubeInfo` gives the linked `sources`, then `getSource` with
  `includeSourceData: true`; Shopify `metafields` hold specs). Leave a gap
  rather than make something up.
- Media: look in the library first (`listFiles`, or `prompt` to search).
  Otherwise import with `importFileFromUrl`, or upload with
  `createFileUpload`, a PUT, then `completeFileUpload`. Prefer 9:16 portrait
  JPG for feed cards. If you generate images, follow the `images` topic,
  including the generation metadata.

## 4. Build each cube

1. `createCubes` with `is_draft: true` (or reuse an existing cube). Set its
   type: `product`, `general` or `home`. Set title, description and handle;
   per-language SEO goes in `upsertCubeView`.
2. Block 0: `createCubeContent` with a `mainfeeditem` first in the array. It
   must have a Rubikc `imageUrl` or `videoUrl`, a short and specific `title`,
   and an optional one-line `description`. This card decides whether anyone
   opens the cube.
3. Detail blocks, placed with `orderIndex`:
   - one complete message per block; don't split a thought across slides
   - one `gallery` instead of many `image` slides
   - `text` with a short preview, and a body that adds to it rather than
     repeating it
   - `cubepromotion` only for real next steps (other cubes, or a structure)
   - product cubes already have add to cart on block 0
4. Before saving a type you have not used yet, check it with
   `previewCubeContent`. Split batches to stay under `maxPayloadBytes`.
5. `listCubeContent` or `listCubeOrders` to confirm the order and that block 0
   is the feed card.

## 5. Reuse and connect

- Content repeated across cubes, such as shipping info, a brand promise or a
  newsletter signup, belongs in a shared block. Create it with
  `createGlobalContent`, or `promoteCubeContentToShared` for an existing
  block. Place it with `insertCubeOrderItem` using `globalContentId` and
  `languages: "all"`.
- Related products: set them with `updateCubeRelations` (`cross-sell`,
  `up-sell`), then place the generated `referenced_products` block. Don't
  hand-pick promotions for this.
- Questions for shoppers: `createSurvey`, then place it with `surveyId`.
  Never create survey blocks with `createCubeContent`.
- Where it shows up: `addCubeToStructure`, the menu (`upsertNavigation`), or
  home pins (`setFeedPinnedCubes`). Only do this if the user wants it.

## 6. Other languages

Read `instructions` topic `locales`. Write translated blocks in the target
language. Then:

1. `copyCubeVersionToLanguage` copies the version layout. It does not
   translate.
2. Swap in the translated blocks.
3. `validateCubeLocalization` finds gaps.

Translate the copy itself. Don't machine-copy the default language into every
version.

## 7. Check and hand over

Go through the quality checklist in the `content` topic. The main points:

- block 0 is a media-backed `mainfeeditem`
- no placeholders and no invented claims
- no repeated text
- no third-party URLs
- the language is right

Publish (`publishCubeContent`, and clear `is_draft`) only if the user asked.

Report each cube's ID and title, its blocks, what is still a draft, and any
facts or media the merchant should supply.
