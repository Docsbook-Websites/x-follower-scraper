---
title: "Export the members of an X List or Community"
description: "Scrape List members, List followers, and public Community members with X Follower Scraper using List URLs, Community URLs, or numeric IDs."
---

A public X List is a segment somebody already curated by hand; a public X Community is a self-selected topic audience. X Follower Scraper addresses both by URL or by numeric ID, and returns the same profile rows it returns for followers.

## Scrape a List by URL

Paste the List URL into `startUrls`. The Actor routes it by path:

```json
{
  "startUrls": [
    { "url": "https://x.com/i/lists/1748648376080666720/members" },
    { "url": "https://x.com/i/lists/1748648376080666720/followers" }
  ],
  "maxItems": 2000
}
```

A List URL with no trailing segment — `https://x.com/i/lists/<id>` — resolves to `list_members`.

## Scrape a Community by URL

```json
{
  "startUrls": [
    { "url": "https://x.com/i/communities/1493446837214187523/members" }
  ],
  "maxItems": 2000
}
```

A Community URL always resolves to `community_members`, with or without the trailing segment.

## Scrape by numeric List or Community ID

```json
{
  "listIds": ["1748648376080666720"],
  "communityIds": ["1493446837214187523"],
  "maxItemsPerTarget": 500,
  "maxItems": 1500
}
```

List IDs default to members. Community IDs always use members. The `relation` field does not change either of them — it applies to handles and numeric user IDs only.

## Mix Lists, Communities, and accounts in one run

One run accepts every target type together, and each row records where it came from:

```json
{
  "twitterHandles": ["openai"],
  "listIds": ["1748648376080666720"],
  "communityIds": ["1493446837214187523"],
  "relation": "followers",
  "dedupeMode": "merge",
  "maxItems": 5000
}
```

With `dedupeMode: "merge"`, a profile appearing in the List and among the followers becomes one row carrying both sources and an `overlapCount` of 2. See [find audience overlap](./find-audience-overlap.md).

## Next steps

- Narrow a List export before it bills: [filter before you pay](./filter-before-you-pay.md).
- Check which URL shape maps to which relation: [URL formats reference](../reference/url-formats.md).
- Export or schedule the result: [export and schedule runs](./export-and-schedule.md).
