---
title: "X Follower Scraper FAQ: cost, limits, and reliability"
description: "Direct answers on what the Actor costs, what stops a run, why results come back short, how retries work, and what happens near a timeout."
---

Answers below come from the Actor's own documentation. Where a figure is not stated by the source, this page says so rather than estimating.

<!-- widget:accordion -->

### What does it cost?

`$0.00015` per delivered profile — `$0.15` per 1,000 — on every Apify plan. One charge per delivered dataset row, covering profiles and at most 1 empty-run diagnostic. No separate Xquik subscription, start fee, or query fee. Apify bills platform usage separately. Full breakdown on the [pricing page](./pricing.md).

### Can I try it before committing to a large export?

Yes. Add the Actor from its Apify Console page and run it with a small `maxItems` and a max cost per run. A 200-row test costs `$0.03` at the stated rate. Apify's own free-credit and plan terms are set by Apify — see [Apify pricing](https://apify.com/pricing).

### Do I need an X API key or an X account?

No. The Actor uses its own infrastructure. No login or credentials are required.

### What stops a run?

Your requested item limit and your Apify spend limit. Apify account and platform limits still apply. The default Apify timeout is `0`, meaning no time limit — the Actor follows every live cursor until the cap or the source ends.

### Why did I get fewer rows than `maxItems`?

Filters such as `minFollowers`, `verifiedOnly`, and `bioContains` apply before rows are written, so they reduce the result below the cap. Relax a filter to return more. `maxItems` is a ceiling, not a target.

### Am I charged for profiles a filter rejected?

No. Filters and duplicate removal run before billing, and rows rejected by the dataset are not billed. See [how billing works](./concepts/how-billing-works.md).

### How many followers can I scrape from one account?

X paginates large accounts in batches. Raise Apify's run time limit to fetch more pages. `maxItemsPerTarget` caps each target individually and does not raise the global limit.

### How fast is it?

Runtime depends on target size, filters, and upstream availability. Automatic cursors request up to 300 profiles per page; older cursors keep a 200-profile limit and restart when expired. Deep filtered runs checkpoint Console progress every 5 pages.

### Does it retry temporary failures?

Yes. Up to 3 attempts per page for timeouts, `429`, and `5xx` responses. It honours `Retry-After` when present and otherwise uses exponential backoff. Hard failures preserve partial results, and the run ends with `completionReason: "partial_failure"` when a target stopped after a read failure.

### What happens near the Apify run time limit?

The Actor adds no shorter deadline of its own. It uses Apify's configured limit and reserves the final 15 seconds to flush profiles, checkpoint pagination, write the run report, and exit. Rows the dataset did not accept are not billed.

### Can I resume where I left off?

Resume-cursor input is not exposed yet. Re-running the same target starts from its first available page.

### Can I run this from the API or on a schedule?

Yes to both. The Actor's Apify page carries Python, JavaScript, and cURL examples, and Apify [scheduling](https://docs.apify.com/platform/schedules) runs it on a cron. See [export and schedule runs](./guides/export-and-schedule.md).

### Which build should integrations use?

`latest`. Store runs already use the Actor's `latest` build configuration; API clients should omit the build override or pass `build=latest`. Pinned builds never move automatically, so update any Task or integration that pins an older one.

### Is this affiliated with X Corp?

No. Xquik is an independent third-party service. "Twitter" and "X" are trademarks of X Corp.

### Where do I report a problem?

Use the Issues tab on the Actor's Apify page.

<!-- /widget -->

## Next steps

- Costs in detail: [pricing](./pricing.md).
- Every input field and alias: [input reference](./reference/input.md).
- Run outcomes and diagnostics: [run report reference](./reference/run-report.md).

<!-- widget:cta -->

**Question answered?**

## Run it against one handle and check

The cheapest way to resolve a remaining doubt is a 200-row run with a max cost per run set.

[Open the Actor in Apify Console](https://console.apify.com/actors/AaT0BcKU5GQh97wdt?addFromActorId=AaT0BcKU5GQh97wdt) · [Read getting started](./getting-started.md)

<!-- /widget -->
