---
title: "Find the audience overlap between several X accounts"
description: "Use merge mode in X Follower Scraper to return one row per unique profile with every matching source target and an overlap count attached."
---

Merge mode answers a question a single-account export cannot: which people follow all of these accounts. Set `dedupeMode: "merge"` and the run returns one row per unique profile, carrying every source that matched it.

## Compare three competitors in one run

```json
{
  "twitterHandles": ["openai", "anthropicai", "GoogleDeepMind"],
  "relation": "followers",
  "dedupeMode": "merge",
  "maxItemsPerTarget": 5000,
  "maxItems": 15000
}
```

Keep `maxItems` high enough that every target can contribute rows, and use `maxItemsPerTarget` to control depth per account. A global cap that is too low means the first target consumes it and the comparison has nothing to compare.

## What merge mode adds to each row

| Field | Contains |
|---|---|
| `sourceTargets` | Every target that matched this profile |
| `sourceRelations` | Every relation that matched this profile |
| `sourceUrls` | Every source URL the profile was discovered on |
| `sourceTargetKeys` | Relation-target pairs, for example `followers:nasa` |
| `overlapCount` | Number of matching relation-target pairs |

A row looks like this:

```json
{
  "schemaVersion": 1,
  "id": "44196397",
  "username": "elonmusk",
  "sourceTargets": ["nasa", "spacex"],
  "sourceRelations": ["followers"],
  "sourceUrls": [
    "https://x.com/nasa/followers",
    "https://x.com/spacex/followers"
  ],
  "sourceTargetKeys": ["followers:nasa", "followers:spacex"],
  "overlapCount": 2
}
```

Sort the export by `overlapCount` descending and the top of the list is the part of the category's audience that follows everyone.

## Choosing between `first` and `merge`

| `dedupeMode` | Result | Use it when |
|---|---|---|
| `first` | One row per profile, keeping the first match only | You want a clean deduplicated lead list and do not care which target produced it |
| `merge` | One row per profile with all sources attached | You are comparing audiences and the overlap itself is the answer |

Both remove repeats before writing, so neither bills you twice for the same profile.

## Overlap across relations, not just accounts

Merge mode counts relation-target pairs, so it also compares relation types on one account. Running `relations: ["followers", "following"]` against a single handle with `dedupeMode: "merge"` gives every mutual an `overlapCount` of 2.

## Next steps

- Narrow the merged set before it bills: [filter before you pay](./filter-before-you-pay.md).
- Take the export into a spreadsheet or CRM: [export and schedule runs](./export-and-schedule.md).
- All merge fields in context: [output fields](../reference/output-fields.md).
