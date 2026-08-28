---
title: "Export X profile data and schedule repeat runs"
description: "Take X Follower Scraper results out as CSV, JSON, Excel, or HTML, run the Actor from the Apify API, and schedule repeat runs to track change."
---

Every X Follower Scraper run writes to an Apify dataset. From there the data leaves as a file, through the Apify API, or on a schedule you set.

## Export the dataset

Open the run's Dataset tab and export as JSON, CSV, Excel, or HTML.

Strip diagnostic rows before importing anywhere. A no-input, invalid-input, or zero-output run writes one row with `resultType: "diagnostic"`:

```javascript
const profiles = dataset.filter(r => r.resultType !== "diagnostic");
```

For a CRM import, the columns that usually matter are `username`, `name`, `description`, `followers`, `location`, `url`, and `sourceTarget`. See [output fields](../reference/output-fields.md) for the rest.

## Run the Actor from the API

The Actor's Apify page carries ready examples for Python, JavaScript, and cURL under its API tab. Two rules keep an integration stable:

- **Do not pin a build.** Omit the build override or pass `build=latest`. Pinned builds never move automatically, so an old pin silently keeps old behaviour.
- **Use canonical field names.** Compatibility aliases still work in JSON, API, SDK, automation, and Task inputs, but new integrations should use the canonical names listed in the [input reference](../reference/input.md).

## Schedule repeat runs

Use Apify [scheduling](https://docs.apify.com/platform/schedules) to run the Actor on a cron, and store each run's dataset.

Comparing profile `id` values between two stored datasets gives you arrivals and departures for that audience. The Actor does not emit follower-list change events; Xquik monitors emit supported tweet and profile events instead.

For a scheduled run, set the spend cap rather than a row cap. Leaving `maxItems` empty lets each run return as many profiles as `maxTotalChargeUsd` allows, which keeps a recurring job's cost fixed even as the audience grows.

## Save a run as a Task

Apify Tasks store an input so you can re-run it without retyping. The Actor's page publishes 50 public tasks, each with a bounded input and a matching dataset view — open one, edit the target, and run it as your own.

If you already own Tasks that pin an older build, update them so they follow `latest`.

## Console input controls

The Console form exposes validated controls rather than free text:

- Start URLs accepts URL strings or `{ "url": "..." }` objects, and its JSON editor preserves both API formats.
- Relation, Output Mode, and Dedupe Mode are validated selects.
- Relations is a validated multi-select for multi-relation runs.
- Result limits accept whole numbers of 1 or more; numeric profile filters accept whole numbers of 0 or more.

The visual form hides aliases that duplicate a canonical control. Existing JSON and saved Task inputs keep their current behaviour.

## Next steps

- Keep a recurring job's cost predictable: [pricing](../pricing.md).
- Understand what a partial run means for your data: [run report reference](../reference/run-report.md).
- Decide whether a REST API fits better than a scheduled Actor: [Actor or API](../concepts/actor-vs-api.md).
