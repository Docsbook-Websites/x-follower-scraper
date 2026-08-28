---
title: "X Follower Scraper input reference"
description: "Every input field the Actor accepts: targets, relations, limits, dedupe and output modes, profile filters, and the compatibility aliases for each."
---

All fields are optional except that a run must supply at least one of `startUrls`, `twitterHandles`, `userIds`, `listIds`, or `communityIds`, or one of their documented aliases. Use canonical names in new integrations; aliases remain available in JSON, API, SDK, automation, and Task inputs.

## Targets

| Field | Type | Aliases | Notes |
|---|---|---|---|
| `startUrls` | array | — | URL strings or `{ "url": "..." }` objects. Routed by path — see [URL formats](./url-formats.md) |
| `twitterHandles` | string[] | `username`, `usernames`, `user_names` | With or without a leading `@` |
| `userIds` | string[] | `twitterUserIds`, `user_ids` | Numeric X user IDs |
| `listIds` | string[] | — | Numeric List IDs. Default to members |
| `communityIds` | string[] | — | Numeric Community IDs. Always members |

## Relations

| Field | Type | Accepted values |
|---|---|---|
| `relation` | string | `followers`, `following`, `verified_followers` |
| `relations` | string[] | Any combination of the above, for multi-relation runs |

`relation` applies to handles and numeric user IDs. It does not change List or Community targets.

Boolean equivalents are also accepted: `getFollowers`, `getFollowing`, `getVerifiedFollowers`, `getListMembers`, `getListFollowers`, `getCommunityMembers`.

## Limits

| Field | Type | Effect |
|---|---|---|
| `maxItems` | number | Global cap on delivered rows. Leave empty to let the spend cap decide |
| `maxItemsPerTarget` | number | Per-target cap, so one large target does not consume the global limit |
| `maxTotalChargeUsd` | number | Hard spend cap. Exposed to the Actor as `ACTOR_MAX_TOTAL_CHARGE_USD` |

Result limits accept whole numbers of 1 or more. In Apify Console, `maxTotalChargeUsd` is the **Max cost per run** field.

## Output and dedupe modes

| Field | Type | Accepted values | Aliases |
|---|---|---|---|
| `outputMode` | string | `compact` (default), `full`, `raw` | `outputVariant` |
| `includeRaw` | boolean | Adds a `raw` copy of the safe source profile | Output Mode alias |
| `dedupeMode` | string | `first`, `merge` | `dedupeAcrossTargets` |

`outputMode: "full"` returns optional profile fields such as pinned tweet IDs, entities, and profile metadata when available. `outputMode: "raw"` adds a sanitized `raw` object alongside the normalized fields.

## Profile filters

Every filter runs before a profile enters the dataset. Numeric filters accept whole numbers of 0 or more.

| Field | Type | Keeps a profile when |
|---|---|---|
| `minFollowers` | number | Follower count is at least this |
| `minFollowing` | number | Following count is at least this |
| `maxFollowing` | number | Following count is at most this |
| `minStatuses` | number | Total posts are at least this |
| `maxStatuses` | number | Total posts are at most this |
| `minAccountAgeDays` | number | Account age reaches this many days |
| `verifiedOnly` | boolean | Publicly Blue or legacy verified |
| `verifiedType` | string | Type is `blue`, `business`, `government`, or `none` |
| `usernameContains` | string | Handle contains the term |
| `bioContains` | string | Bio contains any comma- or newline-separated term, case-insensitive |
| `locationContains` | string | Self-reported location contains the term |
| `hasWebsite` | boolean | Profile carries a website URL |
| `hasLocation` | boolean | Profile carries a location |

## Build selection

Store runs use the Actor's `latest` build configuration. API clients should omit the build override or pass `build=latest`. Pinned builds never move automatically.

## Related

- [URL formats](./url-formats.md) — which URL shape maps to which relation
- [Output fields](./output-fields.md) — what a delivered row contains
- [Filter before you pay](../guides/filter-before-you-pay.md) — the filters applied in practice
