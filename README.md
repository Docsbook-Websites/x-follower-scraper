---
title: "X Follower Scraper: export X audiences at $0.15 per 1,000"
description: "Export X (Twitter) followers, following, verified followers, List members and Community members as structured rows, billed at $0.15 per 1,000 delivered profiles."
---

X Follower Scraper is an Apify Actor built by Xquik. It returns available public profile data for followers, following, Lists and Communities on X (Twitter). Every Apify plan pays the same rate: `$0.00015` per delivered profile, which is `$0.15` per 1,000. Apify bills platform usage separately. There is no X login, no start fee, and no query fee.

> Xquik is an independent third-party service. It is not affiliated with X Corp. "Twitter" and "X" are trademarks of X Corp.

## You pay for rows you keep, not rows the Actor reads

Filters and duplicate removal in X Follower Scraper run **before** billing. A run that inspects 40,000 follower profiles and writes 500 that match `minFollowers: 1000` and `verifiedOnly: true` bills 500 rows — `$0.075` at the stated rate. Rows rejected by the dataset are not billed.

That single behaviour is what separates a targeted audience export from a bulk dump you pay for twice: once to collect, once to clean. See [how billing works](./concepts/how-billing-works.md) for the full mechanism.

## What one run can cover

One run accepts handles, numeric IDs, full URLs, and short paths, and mixes them freely. The Actor routes each target to its relation:

| Relation | Source |
|---|---|
| `followers` | An account's followers |
| `following` | The accounts a user follows |
| `verified_followers` | An account's verified followers |
| `list_members` | Members of a public X List |
| `list_followers` | Followers of a public X List |
| `community_members` | Members of a public X Community |

Every output row carries `sourceTarget`, `sourceRelation`, and `sourceUrl`, so a merged export of twelve competitors still tells you where each profile came from.

## Start here

<!-- widget:cards -->

- [Run your first export](./getting-started.md) — Paste one handle and get a CSV of matching profiles {rocket}
- [What it costs](./pricing.md) — The rate, worked examples, and how to hard-cap a run {credit-card}
- [Filter before you pay](./guides/filter-before-you-pay.md) — The 13 filters that run ahead of billing {filter}
- [Find audience overlap](./guides/find-audience-overlap.md) — One row per profile across several competitors {users}
- [Input reference](./reference/input.md) — Every field, alias, and accepted value {sliders-horizontal}
- [Output fields](./reference/output-fields.md) — What each row contains {table}

<!-- /widget -->

## What the Actor does not return

Viewer-relative state belongs to Xquik's fetch account, not to your dataset. Follow, block, mute, DM, and notification flags are always removed, including from raw output. Resume-cursor input is not exposed yet: re-running a target starts from its first available page.

Results can contain personal data, including self-reported locations. Confirm a lawful purpose and follow applicable privacy rules before you export. Ask qualified counsel when uncertain.

## Next steps

- New to the Actor: [run your first export](./getting-started.md).
- Evaluating cost: [pricing](./pricing.md) and [how billing works](./concepts/how-billing-works.md).
- Deciding between the Actor and Xquik's REST API: [Actor or API](./concepts/actor-vs-api.md).

<!-- widget:cta -->

**Pay per delivered profile**

## Run it on your own audience

Add the Actor in Apify Console, paste one handle, and see real rows before you commit to a large export.

[Try X Follower Scraper](https://console.apify.com/actors/AaT0BcKU5GQh97wdt?addFromActorId=AaT0BcKU5GQh97wdt) · [Read the pricing page](./pricing.md)

<!-- /widget -->
