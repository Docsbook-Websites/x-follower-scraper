---
title: "Filter X profiles before they enter your dataset"
description: "Use the 13 profile filters in X Follower Scraper to reject profiles ahead of billing, so a run charges only for rows that match your criteria."
---

Filters in X Follower Scraper run before a profile enters your dataset, and rows rejected by the dataset are not billed. The Actor may inspect far more profiles than it writes; you pay for what passes.

## The filters available

| Filter | Type | Keeps a profile when |
|---|---|---|
| `minFollowers` | number | Its follower count is at least this |
| `minFollowing` | number | Its following count is at least this |
| `maxFollowing` | number | Its following count is at most this |
| `minStatuses` | number | Its total posts are at least this |
| `maxStatuses` | number | Its total posts are at most this |
| `minAccountAgeDays` | number | The account is at least this old |
| `verifiedOnly` | boolean | It is publicly Blue or legacy verified |
| `verifiedType` | string | Its type is `blue`, `business`, `government`, or `none` |
| `usernameContains` | string | Its handle contains the term |
| `bioContains` | string | Its bio contains any supplied term |
| `locationContains` | string | Its self-reported location contains the term |
| `hasWebsite` | boolean | Its profile carries a website URL |
| `hasLocation` | boolean | Its profile carries a location |

Numeric filters accept whole numbers of 0 or more.

## Combine filters into one audience definition

Every filter must pass for a row to be written and billed:

```json
{
  "twitterHandles": ["openai"],
  "relation": "followers",
  "minFollowers": 1000,
  "verifiedOnly": true,
  "verifiedType": "business",
  "minStatuses": 100,
  "usernameContains": "ai",
  "bioContains": "founder, CEO",
  "locationContains": "San Francisco",
  "maxItems": 500
}
```

## Match several bio terms at once

Separate `bioContains` alternatives with commas or new lines. A profile passes when its bio contains **any** supplied term, and matching is case-insensitive.

```json
{
  "bioContains": "head of growth, VP marketing, demand gen"
}
```

This is an OR across terms, not an AND. To require two conditions, put one in `bioContains` and the other in a different filter such as `locationContains`.

## How `verifiedOnly` treats conflicting flags

`verifiedOnly` accepts public Blue and legacy verified profiles. Where source flags disagree, a false value never hides a true verification state — a profile verified by either signal passes.

## Remove duplicates before they bill

When a run has several targets, the same profile can appear more than once. `dedupeMode` decides what happens:

- `dedupeMode: "first"` keeps only the first matching profile row and drops the repeats before writing.
- `dedupeMode: "merge"` keeps one row per profile with every matching source attached.

Either way, repeats are removed before writing, so they are not billed. `dedupeAcrossTargets` is an accepted alias for the same control.

## When a filtered run returns nothing

A zero-output run writes one diagnostic row with `resultType: "diagnostic"` and a `status` of `zero-output`. That row is billable as the run's single diagnostic. Filter diagnostics out of your data with:

```javascript
dataset.filter(r => r.resultType !== "diagnostic")
```

If a run returns fewer rows than expected, relax the strictest filter first — usually `verifiedOnly` or a high `minFollowers`.

## Next steps

- Understand what the filtering saves you: [how billing works](../concepts/how-billing-works.md).
- Apply filters across several competitors: [find audience overlap](./find-audience-overlap.md).
- Field names and accepted values: [input reference](../reference/input.md).
