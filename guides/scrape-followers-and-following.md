---
title: "Scrape followers, following, and verified followers on X"
description: "Target one account or many with X Follower Scraper using handles, numeric IDs, or URLs, and pull several relations in a single run."
---

X Follower Scraper takes four kinds of target for a user account: handles, numeric user IDs, full URLs, and short paths. All of them can appear in the same run. This guide covers the three account relations — `followers`, `following`, and `verified_followers`.

## Scrape several handles at once

`twitterHandles` is shorthand for many `/<handle>/followers` targets. Usernames are accepted with or without a leading `@`.

```json
{
  "twitterHandles": ["elonmusk", "nasa", "openai"],
  "relation": "followers",
  "maxItems": 1000
}
```

`relation` switches what is scraped for every handle in the list: `followers`, `following`, or `verified_followers`. The aliases `username`, `usernames`, and `user_names` accept the same input.

Independent targets run concurrently, and each target keeps ordered cursor pagination.

## Pull more than one relation in a single run

Use `relations` when you want the same accounts scraped several ways:

```json
{
  "usernames": ["nasa"],
  "relations": ["followers", "following"],
  "maxItems": 1000
}
```

Boolean switches do the same job: `getFollowers`, `getFollowing`, `getVerifiedFollowers`, `getListMembers`, `getListFollowers`, and `getCommunityMembers`.

Each output row records which relation produced it in `sourceRelation`, so a two-relation run stays separable after export.

## Paste URLs instead of handles

`startUrls` accepts URL strings or `{ "url": "..." }` objects, and the Actor routes each one to its relation automatically:

```json
{
  "startUrls": [
    { "url": "https://x.com/nasa/followers" },
    { "url": "https://x.com/spacex/verified_followers" },
    { "url": "https://x.com/elonmusk/following" }
  ],
  "maxItems": 5000
}
```

A bare profile URL such as `https://x.com/nasa` uses the run's `relation` value, defaulting to `followers` when unset. The full routing table is in the [URL formats reference](../reference/url-formats.md).

## Scrape by numeric user ID

Numeric IDs survive handle changes, which matters for a target list you reuse across months.

```json
{
  "userIds": ["44196397"],
  "relation": "followers",
  "maxItemsPerTarget": 500,
  "maxItems": 1500
}
```

`relation` applies to numeric user IDs. The aliases `twitterUserIds` and `user_ids` accept the same input.

## Stop one large account from consuming the run

`maxItems` is the global row cap. `maxItemsPerTarget` caps each target separately, which prevents the first large account in the list from spending the whole budget before the others start contributing.

With `maxItemsPerTarget: 500` and `maxItems: 1500`, three targets each contribute at most 500 rows.

## Next steps

- Cut the billed rows down to the profiles you want: [filter before you pay](./filter-before-you-pay.md).
- Compare several accounts in one dataset: [find audience overlap](./find-audience-overlap.md).
- Every field and alias: [input reference](../reference/input.md).
