---
title: "What teams use X Follower Scraper for"
description: "Six jobs the Actor is built for, from competitor lead research to agent-ready audience datasets, each with the input that gets you there."
---

Each job below maps to a run configuration you can copy. Every one of them bills only the rows that survive filtering, so narrowing the audience narrows the invoice.

## Export competitor followers for lead research

You want the people already following a competitor, not everyone who follows them. Filter to a follower floor and a bio keyword so the export arrives ready for a CRM import.

```json
{
  "twitterHandles": ["competitor-handle"],
  "relation": "followers",
  "minFollowers": 500,
  "bioContains": "founder, head of growth, CTO",
  "hasWebsite": true
}
```

See [scrape followers and following](./guides/scrape-followers-and-following.md).

## Compare audiences across several accounts

Merge mode returns one row per unique profile with `sourceTargets`, `sourceRelations`, `sourceUrls`, `sourceTargetKeys`, and `overlapCount` attached. Sorting by `overlapCount` gives you the people who follow every account in the set — usually the densest part of a category's audience.

See [find audience overlap](./guides/find-audience-overlap.md).

## Build an audience dataset for AI agents and RAG

Rows follow a public profile contract with `schemaVersion` on every record, so a dataset scraped this month parses with the same code as one scraped last month. Set `outputMode: "full"` for optional profile metadata, or `outputMode: "raw"` to keep a sanitized copy of the source profile alongside the normalized fields.

See [output fields](./reference/output-fields.md).

## Export the members of a curated List or Community

A well-maintained X List is a hand-built segment somebody else already curated. Community members are a self-selected topic audience. Both are addressable by URL or numeric ID.

See [scrape Lists and Communities](./guides/scrape-lists-and-communities.md).

## Segment an existing follower base

Run the filters against your own account rather than a competitor's: `locationContains` for a market, `verifiedType` for account class, `minAccountAgeDays` to exclude young accounts, `usernameContains` for a naming pattern.

See [filter before you pay](./guides/filter-before-you-pay.md).

## Track how an audience changes over time

Schedule repeat runs with Apify [scheduling](https://docs.apify.com/platform/schedules) and store each dataset. Comparing profile `id` values between two datasets gives you arrivals and departures. The Actor itself does not emit follower-list change events — Xquik monitors emit supported tweet and profile events instead.

See [export and schedule runs](./guides/export-and-schedule.md).

## Before you export personal data

Results can contain personal data, including self-reported locations. Confirm a lawful purpose and follow applicable privacy rules. Ask qualified counsel when uncertain.

## Next steps

- Pick the guide matching your job above, or start with [getting started](./getting-started.md).
- Estimate the bill first: [pricing](./pricing.md).

<!-- widget:cta -->

**Start with the job closest to yours**

## Run one of these on a real audience

Copy any input above, set a max cost per run, and check the rows before scaling the target list.

[Open the Actor in Apify Console](https://console.apify.com/actors/AaT0BcKU5GQh97wdt?addFromActorId=AaT0BcKU5GQh97wdt) · [See the input reference](./reference/input.md)

<!-- /widget -->
