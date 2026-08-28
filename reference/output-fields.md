---
title: "X Follower Scraper output field reference"
description: "Every field a delivered profile row can contain, including source attribution, merge-mode overlap fields, and what the Actor always strips out."
---

Each profile is a JSON object. Compact mode is the default and returns normalized public fields, schema version fields, and source metadata when available. Dataset and run-report schemas describe every returned field, and primitive fields include examples for agents and generated integrations.

## Profile fields

| Field | Description |
|---|---|
| `id` | Numeric X user ID |
| `username` | Handle, without `@` |
| `name` | Display name |
| `description` | Bio text |
| `followers` | Follower count |
| `following` | Following count |
| `statusesCount` | Total posts |
| `mediaCount` | Total media uploaded |
| `favouritesCount` | Total likes given |
| `verified` | Combined public Blue or legacy verified flag |
| `verifiedType` | `blue`, `business`, `government`, or `none` |
| `location` | Self-reported location |
| `url` | Website URL from the profile |
| `profilePicture` | Avatar URL, full size |
| `coverPicture` | Banner URL |
| `createdAt` | Account creation timestamp string from X |

## Source attribution fields

| Field | Description |
|---|---|
| `sourceTarget` | Handle or ID this profile was scraped from |
| `sourceRelation` | `followers`, `following`, `list_members`, and so on |
| `sourceUrl` | Exact URL the profile was discovered on |

## Merge mode fields

Present when `dedupeMode: "merge"` is set.

| Field | Description |
|---|---|
| `sourceTargets` | All targets that matched this profile |
| `sourceRelations` | All relations that matched this profile |
| `sourceUrls` | All source URLs that matched this profile |
| `sourceTargetKeys` | Relation-target pairs, for example `followers:nasa` |
| `overlapCount` | Number of matching relation-target pairs |

## Mode and schema fields

| Field | Description |
|---|---|
| `schemaVersion` | Row schema version |
| `_schema_version` | Same value under the underscore-prefixed name |
| `resultType` | Row type in full and raw output modes, and `diagnostic` on empty runs |
| `raw` | Safe source profile before Actor-specific formatting, when requested |

Rows follow the public profile contract, which covers identity, counts, verification, availability, affiliates, professional data, and biographies. Source attribution, entities, and pinned tweet IDs remain available. Consult the Actor's OpenAPI schema for the exact field set.

## A compact-mode row

```json
{
  "schemaVersion": 1,
  "_schema_version": 1,
  "id": "44196397",
  "username": "elonmusk",
  "name": "Elon Musk",
  "description": "...",
  "followers": 180000000,
  "following": 500,
  "statusesCount": 42000,
  "mediaCount": 3200,
  "favouritesCount": 120000,
  "verified": true,
  "verifiedType": "blue",
  "location": "...",
  "url": "https://...",
  "profilePicture": "https://...",
  "coverPicture": "https://...",
  "createdAt": "Tue Jun 02 20:12:29 +0000 2009",
  "sourceTarget": "nasa",
  "sourceRelation": "followers",
  "sourceUrl": "https://x.com/nasa/followers"
}
```

Sample values are illustrative. Responses reflect source data at run time.

## Fields the Actor always removes

Viewer-relative state belongs to Xquik's fetch account rather than to your dataset. Follow, block, mute, DM, notification, and similar viewer flags are always removed, including from raw output.

## Related

- [Input reference](./input.md) — the fields that produce these rows
- [Run report](./run-report.md) — per-run metadata and diagnostic rows
- [Find audience overlap](../guides/find-audience-overlap.md) — merge fields in use
