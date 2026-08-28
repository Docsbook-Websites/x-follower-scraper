---
title: "X Follower Scraper pricing: $0.15 per 1,000 profiles"
description: "One rate on every Apify plan, what counts as a billable row, worked cost examples, and how to hard-cap the spend of a single run."
---

X Follower Scraper charges `$0.00015` per delivered profile — `$0.15` per 1,000 — on every Apify plan. There is no separate Xquik subscription, no start fee, and no query fee. Apify bills your platform usage separately under your own Apify plan.

## What you are charged for

| Item | Charge |
|---|---|
| A profile row written to your dataset | `$0.00015` |
| An empty-run diagnostic row | `$0.00015`, at most 1 per run |
| Starting a run | No charge |
| Adding targets or selecting relations | No charge |
| A profile the Actor inspected but a filter rejected | No charge |
| A duplicate removed by `dedupeAcrossTargets` | No charge |
| A row the dataset rejected | No charge |

One charge applies per delivered dataset row. The Actor may read many more profiles than it writes; the reading is not billed.

## Worked examples

These are arithmetic on the stated rate, not separate plans:

| Delivered profiles | Xquik charge |
|---|---|
| 1,000 | $0.15 |
| 10,000 | $1.50 |
| 100,000 | $15.00 |
| 1,000,000 | $150.00 |

Apify platform usage is additional and depends on your Apify plan. See [Apify pricing](https://apify.com/pricing) for those terms — this page does not restate them.

## Cap the spend of a run

Set **Max cost per run** in Apify Console, or `maxTotalChargeUsd` through the Apify API. Apify exposes that value to the Actor as `ACTOR_MAX_TOTAL_CHARGE_USD`, and the Actor stops before accepting rows beyond it.

```json
{
  "twitterHandles": ["openai", "anthropicai"],
  "relation": "followers",
  "minFollowers": 5000,
  "maxTotalChargeUsd": 5
}
```

Leave `maxItems` empty to let the run return as many profiles as the spend cap allows. Set `maxItems` only when you want a smaller result cap than the budget would permit, and `maxItemsPerTarget` when one large account would otherwise consume the whole global limit.

## Knowing the cost of a run after it finishes

Every run writes a `run-report` record containing `estimatedChargeUsd`, calculated from the live pay-per-event price Apify exposes to the Actor. Every outcome writes one, including no-input and invalid-input exits. Its `version` field reports the exact published Actor source version, so a cost you compare across weeks is anchored to a known build.

See the [run report reference](./reference/run-report.md) for the fields it carries.

## Partial and interrupted runs

- `completionReason: "partial_failure"` means at least one target stopped after a read failure; `failedTargets` counts them. Accepted profiles remain billable data rows.
- `completionReason: "deadline_reached"` means a finite timeout you set is near. The Actor reserves the final 15 seconds for checkpoints, rows, reports, and a clean exit. Valid profiles are delivered and bill once; unfinished pagination remains resumable by re-running the target.

## Next steps

- Reduce the billed row count before the run starts: [filter before you pay](./guides/filter-before-you-pay.md).
- Understand the mechanism behind the rate: [how billing works](./concepts/how-billing-works.md).
- Common cost questions, answered flatly: [FAQ](./faq.md).

<!-- widget:cta -->

**One rate, every plan**

## Test the rate on a 200-row run

Set a max cost per run, scrape one handle, and compare the `estimatedChargeUsd` in the run report against your own estimate.

[Open the Actor in Apify Console](https://console.apify.com/actors/AaT0BcKU5GQh97wdt?addFromActorId=AaT0BcKU5GQh97wdt) · [Read the getting started guide](./getting-started.md)

<!-- /widget -->
