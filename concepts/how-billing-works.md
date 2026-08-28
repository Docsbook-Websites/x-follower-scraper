---
title: "How pay-per-delivered-row billing works"
description: "Why X Follower Scraper charges for written rows rather than requests, where filtering and deduplication sit in that order, and how to cap a run."
---

Most scraping tools charge for work performed: requests issued, pages fetched, compute consumed. X Follower Scraper charges for output accepted — `$0.00015` per delivered profile row. That difference decides how you should design a run.

## The order of operations

A profile passes through four stages, and only the last one costs money:

1. **Fetch.** The Actor requests a page of up to 300 profiles from the source. Not billed.
2. **Filter.** Every filter you set is evaluated against the profile. A rejection ends its journey. Not billed.
3. **Deduplicate.** With `dedupeMode` set, repeats across targets are removed before writing. Not billed.
4. **Write.** The row enters your dataset. **Billed once.**

Rows rejected by the dataset are not billed either. Starts, targets, and relation selection add no separate query charge.

## Why this changes how you configure a run

Under request-based pricing, a narrow filter costs the same as a broad one — you paid to fetch either way — so the rational move is to export everything and clean it later. Under row-based pricing the incentive reverses: the strictest filter that still captures your audience is also the cheapest run.

A run over a 40,000-follower account that writes 500 rows matching `minFollowers: 1000` and `bioContains: "founder"` bills 500 rows. The other 39,500 profiles were read and discarded at no charge.

This is why the [filter guide](../guides/filter-before-you-pay.md) is a cost document as much as a targeting one.

## Two caps, two different jobs

`maxItems` caps rows. `maxTotalChargeUsd` caps dollars. They are not interchangeable.

- Use `maxTotalChargeUsd` when the budget is the constraint. Apify exposes it to the Actor as `ACTOR_MAX_TOTAL_CHARGE_USD`, and the Actor stops before accepting rows beyond it. Leave `maxItems` empty and the run returns as many profiles as the budget allows.
- Use `maxItems` when you want a smaller result than the budget would permit — a sample, or a fixed-size list.
- Use `maxItemsPerTarget` when several targets share a run and the first should not consume the global limit.

For a scheduled job, capping dollars keeps cost fixed as the audience grows. Capping rows does not.

## What still bills on a run that produced nothing

A no-input, invalid-input, or zero-output run writes one diagnostic row, and a run is billed for at most 1 of them. That is the floor: a misconfigured run costs `$0.00015`, not the price of the profiles it would have written.

## Verifying the charge afterwards

The `run-report` record carries `estimatedChargeUsd`, computed from the live pay-per-event price Apify exposes to the Actor, and `version`, identifying the exact published Actor source that ran. Comparing those two across runs tells you whether a cost change came from your input or from a new build.

Apify bills platform usage separately, under your own Apify plan. The rate on this page covers the Actor's charge only.

## Next steps

- Put the mechanism to work: [filter before you pay](../guides/filter-before-you-pay.md).
- The rate, worked examples, and caps: [pricing](../pricing.md).
- Fields the run report writes: [run report reference](../reference/run-report.md).
