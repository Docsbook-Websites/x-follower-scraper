---
title: "Run report, completion reasons, and diagnostic rows"
description: "What X Follower Scraper records about every run: estimated charge, Actor version, failed targets, completion reasons, and diagnostic row statuses."
---

Every X Follower Scraper run writes a `run-report` record, including runs that exit on no input or invalid input. It is the record to read when a run cost more or returned less than expected.

## Run report fields

| Field | Description |
|---|---|
| `estimatedChargeUsd` | Calculated from the live pay-per-event price Apify exposes to the Actor |
| `version` | The exact published Actor source version the run used |
| `failedTargets` | Count of targets that stopped after a read failure |
| `completionReason` | Why the run ended — see below |

## Completion reasons

| Value | Meaning |
|---|---|
| `partial_failure` | At least one target stopped after a read failure. Accepted profiles remain billable data rows |
| `deadline_reached` | A finite timeout set by the caller is near |

The default Apify timeout is `0`, so runs have no time limit unless a caller sets one. When a finite limit is near, the Actor reserves the final 15 seconds for checkpoints, rows, reports, and a clean exit. Valid profiles are delivered and bill once, and unfinished pagination remains resumable by re-running the target.

Fast server-side pagination follows the same reporting contract.

## Diagnostic rows

No-input, invalid-input, and zero-output runs write 1 row with `resultType: "diagnostic"` and a `status` field.

| `status` | Cause |
|---|---|
| `no-input` | The run supplied no target |
| `invalid-input` | The input failed validation |
| `zero-output` | The run completed but no profile passed the filters |

A run is billed for at most 1 empty-run diagnostic. Exclude diagnostics from downstream data:

```javascript
dataset.filter(r => r.resultType !== "diagnostic")
```

## Retries and checkpoints

The Actor makes up to 3 attempts per page for timeouts, `429`, and `5xx` responses. It honours `Retry-After` when present and otherwise uses exponential backoff. Hard failures preserve partial results.

Checkpoints preserve accepted rows, timing, and failure counts after restarts. Deep filtered runs checkpoint Console progress every 5 pages, which reduces non-data traffic between page fetches.

## Pagination and page timing

Automatic cursors request up to 300 profiles per page. Older cursors keep their 200-profile limit and restart when expired.

Each page log reports `fetchDurationMs`, `processingDurationMs`, `pushDurationMs`, `statusDurationMs`, and `fullPageDurationMs`, without repeating targets.

Independent targets run concurrently while each target keeps ordered cursor pagination. Dataset writes keep caps, deduplication, attribution, and billing atomic.

## Related

- [Output fields](./output-fields.md) — the rows a successful run writes
- [How billing works](../concepts/how-billing-works.md) — what turns a row into a charge
