---
name: rubikc-optimize
description: Audit and improve an existing Rubikc Space. Covers content quality checks (missing feed images, placeholder or repeated copy, drafts, broken menu links, missing translations), data-driven fixes to weak cubes, refreshing imported product cubes from their Sources, and A/B testing cube Versions with traffic splits. Use when the user asks what to fix, wants weak cubes improved, a content audit, or a test of two versions.
---

# Optimize a Space

Goal: find the changes most likely to matter, make them as reviewable drafts,
and prove the result. Where it makes sense, prove it with a traffic split.

Follow the `rubikc` skill for startup and safety rules. For metric meanings,
read `instructions` topic `analytics`. For content rules, read topic `content`.

## 1. Audit (read only)

Run both passes, then merge them into one ranked list.

**Content quality.** Use `getCubeInventory` and page through every cube; don't
loop over `getCubeInfo`. Flag:

- feed-visible cubes whose block 0 is not a `mainfeeditem`, or has no
  image or video
- media hosted outside Rubikc
- placeholder text, repeated preview and body text, generic filler
- blocks still unpublished, and cubes still draft with no `publish_at`
- empty or thin product cubes with nothing beyond the feed card
- menu links to missing or draft cubes (`listNavigation`, `getStorefrontMap`)
- languages with gaps (`getLocales`, then `validateCubeLocalization` on key
  cubes)
- Sources still pending in the Inbox, or failed transforms
  (`getSourcesInboxSummary`, `listSourceTransformRuns`)

**Performance.** Over the last 7–30 days:

- `rankCubes` and `analyzeCubeContribution` for the cubes that matter
- `analyzeLandingCubes` for entry points that bounce
- `analyzeCubeSlideRetention` or `analyzeCubeBlocks` on high-traffic cubes,
  to find the block where people leave
- `analyzeFeedEffectiveness` for feed cards that are seen but not opened

Rank each issue by reach (traffic, or feed visibility) times how bad it is.
Present the top items with evidence, and confirm which ones to fix before
writing anything.

## 2. Fix

- **Weak feed card** (seen, rarely opened): better media, and a sharper title
  and description on block 0. Use `updateCubeContent`, with media from
  `importFileFromUrl`.
- **Early drop-off inside a cube**: tighten or reorder the blocks
  (`moveCubeOrderItem`), merge fragments, cut slides that repeat each other,
  and move the strongest proof forward.
- **Dead end**: add a next step. That can be related products (set with
  `updateCubeRelations`, then place `referenced_products`), or a
  `cubepromotion` to the right cube or structure.
- **Stale imported product cube**: `getCubeInfo` gives the linked `sources`.
  Read `instructions` topic `sources`, then `transformSource` (dry run first)
  with only the `processSteps` you need. If blocks the merchant edited were
  kept, read the `preserve_edits` step in that topic.
- **Quality issues**: fix them in place with `updateCubeContent`. Keep the
  fields you are not changing. Facts come only from Sources or the merchant.
- **Feed order**: `setFeedPinnedCubes` for home pins. Set `exclude_from_feed`
  for cubes meant only for direct links.

Write every fix as an unpublished draft. Publish only when the user asks.
`listCubeContentVersions` and `restoreCubeContentVersion` undo a change.

## 3. Test with a traffic split

Use a split when the change is a judgment call, such as a new feed card,
order or angle, and the cube gets enough traffic to tell. A useful split needs
at least a few hundred sessions per Version.

1. Read `instructions` topic `content` for Versions, then
   `listCubeVersionTests` and `listCubeOrders` for the cube and language.
2. `duplicateCubeVersion` with `cloneBlocks: true`, so edits stay on the new
   Version. Make the change on the copy only.
3. `saveCubeVersionTest`: two to four Versions, equal or weighted, as a dry
   run first. **Ask before `startCubeVersionTest`**, because it changes what
   shoppers see.
4. Later, `compareCubeVersionTest` shows impressions, opens, dwell, add to
   cart and purchases per Version. Call a winner only when the difference is
   clear and the sample is large. Otherwise say it is too early.
5. **Ask before** `completeCubeVersionTest` (keeps the winner live under the
   same link) or `cancelCubeVersionTest`.

Only one split can run per cube and language. Returning visitors stay on the
Version they were assigned.

## 4. Report

- What you audited, and the period.
- A ranked list of issues, with evidence.
- What you changed: cube IDs and titles, before and after for the key copy.
  Say what is still a draft.
- Splits created or running, and when to check back.
- What still needs the merchant: photos, facts, or a decision to publish.
