---
title: "Accepted X URL formats and their relations"
description: "The full routing table X Follower Scraper uses to map profile, List, and Community URLs and short paths to a scrape relation."
---

X Follower Scraper routes each entry in `startUrls` to a relation by its path shape. Full URLs and short paths both work, and `twitter.com` and `mobile.twitter.com` are accepted everywhere `x.com` is.

## Full URLs

| URL | Relation |
|---|---|
| `https://x.com/<handle>/followers` | `followers` |
| `https://x.com/<handle>/verified_followers` | `verified_followers` |
| `https://x.com/<handle>/following` | `following` |
| `https://x.com/<handle>` | The run's `relation`, defaulting to `followers` |
| `https://x.com/i/lists/<id>/members` | `list_members` |
| `https://x.com/i/lists/<id>/followers` | `list_followers` |
| `https://x.com/i/lists/<id>` | `list_members` |
| `https://x.com/i/communities/<id>/members` | `community_members` |
| `https://x.com/i/communities/<id>` | `community_members` |

## Short paths

| Path | Relation |
|---|---|
| `<handle>/followers` | `followers` |
| `<handle>/following` | `following` |
| `<handle>/verified_followers` | `verified_followers` |
| `lists/<id>/members` | `list_members` |
| `lists/<id>/followers` | `list_followers` |
| `communities/<id>/members` | `community_members` |

## Accepted entry shapes

`startUrls` accepts either form:

```json
{
  "startUrls": [
    "nasa/followers",
    { "url": "https://x.com/i/lists/1748648376080666720/members" }
  ]
}
```

The Console's JSON editor preserves both API formats.

## Related

- [Input reference](./input.md) — targets, relations, and limits
- [Scrape Lists and Communities](../guides/scrape-lists-and-communities.md) — these URLs in a full run
