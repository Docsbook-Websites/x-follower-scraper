---
title: "Choosing between the Apify Actor and the Xquik REST API"
description: "When a pay-per-result Apify Actor is the right tool for X audience data and when Xquik's REST API, webhooks, or MCP server fits the job better."
---

Xquik ships the same audience data through two surfaces. X Follower Scraper is an Apify Actor: you configure a run, it writes a dataset, you export it. The Xquik platform is a REST API with 47 dashboard tools, 128 REST operations, signed webhooks, and an MCP server. Neither replaces the other.

## What the Actor is good at

The Actor is a batch job with billing attached. It suits work shaped like a job rather than a request:

- One-off or scheduled exports where the output is a file or a dataset.
- Runs where you want a hard spend cap enforced by the platform.
- Work done by someone who should not hold an API key — the Console form is a validated UI.
- Pipelines already built on Apify integrations, storage, and scheduling.

It charges per delivered row and needs no X login, no credentials, and no infrastructure of yours.

## What the REST API is good at

The API suits work shaped like a request rather than a job:

- Fetching a single account's followers inside your own application flow.
- Reacting to events, using signed webhooks for supported tweet and profile events.
- Giving an AI agent tool access, through the MCP server's supported JSON or text operations.

The relevant endpoints are documented at:

- [Xquik API introduction](https://docs.xquik.com/introduction)
- [Followers API](https://docs.xquik.com/api-reference/x/followers)
- [Following API](https://docs.xquik.com/api-reference/x/following)
- [List Members API](https://docs.xquik.com/api-reference/x/list-members)
- [MCP server overview](https://docs.xquik.com/mcp/overview)
- [Webhooks overview](https://docs.xquik.com/webhooks/overview)

## A rule of thumb

If the answer to "where does the data land" is a dataset, a spreadsheet, or a CRM import, use the Actor. If the answer is "inside my application, at request time", use the API.

One case sits between them: tracking how an audience changes. The Actor handles it by scheduling repeat runs and comparing stored datasets by profile `id`. It does not emit follower-list change events — Xquik monitors emit supported tweet and profile events instead. If you need push rather than poll for those events, that is the API's job.

## Related Xquik Actors

The same pay-per-result model covers neighbouring data:

- [X Tweet Scraper](https://apify.com/xquik/x-tweet-scraper) — tweets, engagement metrics, author profiles, and media
- [X List Scraper](https://apify.com/xquik/x-list-scraper) — List posts, members, and followers
- [X Community Scraper](https://apify.com/xquik/x-community-scraper) — Community info, posts, searches, members, and moderators
- [X Reply Scraper](https://apify.com/xquik/x-reply-scraper) — replies, comments, and reply timelines

## Next steps

- Staying with the Actor: [export and schedule runs](../guides/export-and-schedule.md).
- Comparing the cost model first: [how billing works](./how-billing-works.md).
