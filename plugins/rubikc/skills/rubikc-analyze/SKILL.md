---
name: rubikc-analyze
description: Explain what is happening in a Rubikc Space using its analytics. Use for health checks, weekly or monthly reports, "how is my storefront doing", traffic drops or spikes, conversion and funnel questions, which cubes or blocks work, where visitors land, which traffic sources or countries convert, survey results, A/B test results, or replaying a single visit. Read only; it changes nothing.
---

# Analyze a Space

Goal: a short, honest answer to the merchant's question, backed by numbers,
with the next action for each finding. This skill only reads. To act on what it
finds, use `rubikc-optimize`.

Follow the `rubikc` skill for startup.

## 1. Set up

1. `getMcpStatus`, then `listStorefronts`, and pick the `storefrontId`.
2. `instructions` topic `analytics`. It defines every metric and lists which
   tool answers which question. Read it before interpreting any numbers.
3. Pick the period. With no dates, tools use a rolling window, usually the
   last 7 days compared with the 7 before. For explicit ranges, use ISO
   datetimes (`2026-05-12T00:00:00.000Z`).
4. Before filtering by segment, call `listAnalyticsSegmentOptions` and use
   only keys and values it returns.

## 2. Start broad, then drill down

Start here unless the question is narrow:

1. `analyzeStorefrontOverview`: headline KPIs and change against the previous
   period.
2. `analyzeStorefrontTimeSeries`: when it changed. Look for a step, a spike,
   or a slow drift.
3. `analyzeFunnel` with `comparePreviousPeriod`: from visit to cube to cart to
   checkout to purchase, and where the drop is.

Then follow the drop:

| Question | Tools |
|---|---|
| Which cubes do the work? | `rankCubes` (leaderboard), `analyzeCubeContribution` (cart and purchase credit) |
| Where do visitors enter? | `analyzeLandingCubes` |
| Does the home feed pull people in? | `analyzeFeedEffectiveness` |
| Which traffic is good or bad? | `analyzeTrafficQuality`, then `analyzeTrafficQualityDetail` for one segment |
| Who converts? | `analyzeSessionDemographics` with `actionTypes`, `analyzeBehaviorSegments` |
| Where inside a cube do people leave? | `analyzeCube`, `analyzeCubeBlocks`, `analyzeCubeSlideRetention`, `analyzeCubeBlockPositions` |
| Do cube promotions get clicked? | `analyzeCubePromotionClicks` |
| What did shoppers answer? | `listSurveys`, `getSurveyResults`, `analyzeSurveyOutcomes` |
| Which A/B test version wins? | `listCubeVersionTests`, `compareCubeVersionTest` |
| What did one visitor do? | `listSessions`, then `replaySession` (needs both `userId` and `sessionId`) |
| Is an event even being tracked? | `analyzeActionStats` |

To explain a cube's numbers, also read its content: `getCubeInventory` or
`getCubeInfo`. Then you can name the block that loses people.

## 3. Read the numbers correctly

- Block 0 is the feed card (`mainviews`). Blocks 1+ are cube detail
  (`views`). They are not the same metric.
- Visitors can enter at any cube and any block, not only block 0.
- Product cubes have add to cart on the feed card, so low add to cart is not
  caused by visitors missing a later slide.
- Bots are excluded by default. A consent "reject" is not a bounce, because
  Rubikc still records the session.
- Under about 20 sessions, rates are noise. Say so instead of drawing a
  conclusion.
- Empty results usually mean the wrong date range. Widen it before you say
  there is no data.
- Never invent a country, cause or number. If the data can't answer the
  question, say what would.

## 4. Report

Keep it short and lead with the answer:

1. **Headline**: one or two sentences on the state of the Space and the
   biggest change, with numbers and the period.
2. **Findings**: three to six. Each one has what happened, the evidence
   (metric, period, comparison), and the likely reason, labelled as a guess
   when it is one.
3. **Recommended actions**: specific cubes, blocks or segments, ordered by
   expected impact. Offer to carry them out with `rubikc-optimize`.
4. **Caveats**: small samples, short periods, missing tracking.

Use the merchant's words: Space, cubes, blocks, feed, Versions. Give cube
titles alongside cube IDs.
