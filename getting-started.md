---
title: "Run your first X follower export in about five minutes"
description: "Add X Follower Scraper in Apify Console, scrape one account's followers with a spend cap, and export the matching profiles to CSV."
---

This tutorial takes you from an empty Apify Console to a CSV of X follower profiles filtered to the accounts you actually want. You will scrape one target, cap the spend, and export the result.

**Prerequisites**

- An Apify account. No X (Twitter) account, login, or API key is required.
- One public X handle to scrape, for example `openai`.
- About five minutes, most of it waiting for the run.

<!-- widget:stepper -->

### Add the Actor to your account

Open the [X Follower Scraper page in Apify Console](https://console.apify.com/actors/AaT0BcKU5GQh97wdt?addFromActorId=AaT0BcKU5GQh97wdt) and add it. Store runs use the Actor's `latest` build, so leave the build field alone.

### Enter one target

Switch the input to its JSON editor and paste:

```json
{
  "twitterHandles": ["openai"],
  "relation": "followers",
  "maxItems": 200
}
```

`twitterHandles` accepts handles with or without a leading `@`. `relation` decides what is scraped for every handle in the list: `followers`, `following`, or `verified_followers`.

### Cap what the run can spend

Set **Max cost per run** in Console, or `maxTotalChargeUsd` through the Apify API. Apify exposes that limit to the Actor as `ACTOR_MAX_TOTAL_CHARGE_USD`, and the Actor stops before accepting rows beyond it.

At the stated rate of `$0.00015` per delivered profile, a 200-row run costs `$0.03`. Setting the cap now means a mistyped `maxItems` later cannot surprise you.

### Add a filter so you pay for fewer rows

Filters run before a profile enters your dataset, so a filtered run bills fewer rows than an unfiltered one over the same source. Add two:

```json
{
  "twitterHandles": ["openai"],
  "relation": "followers",
  "minFollowers": 1000,
  "bioContains": "founder, CEO",
  "maxItems": 200
}
```

The Actor may inspect more profiles than it writes. You pay only for rows that pass every filter.

### Start the run and watch the log

Click **Start**. Deep filtered runs checkpoint Console progress every 5 pages. Each page log reports `fetchDurationMs`, `processingDurationMs`, `pushDurationMs`, `statusDurationMs`, and `fullPageDurationMs`.

The default Apify timeout is `0`, meaning no time limit — the Actor follows every live cursor until the cap or the source ends.

### Export the dataset

Open the run's **Dataset** tab and export as JSON, CSV, Excel, or HTML.

If the run produced no matching profiles, the dataset holds one diagnostic row instead. Filter those out in code with `dataset.filter(r => r.resultType !== "diagnostic")`.

<!-- /widget -->

## Why your run returned fewer rows than `maxItems`

Filters such as `minFollowers`, `verifiedOnly`, and `bioContains` apply before writes, so a strict filter set returns fewer rows than the cap allows. Relax a filter to return more results. `maxItems` is a ceiling, never a target.

## Next steps

- Scrape several accounts at once: [scrape followers and following](./guides/scrape-followers-and-following.md).
- Cut the billed row count further: [filter before you pay](./guides/filter-before-you-pay.md).
- Look up any field you saw in the output: [output fields](./reference/output-fields.md).
