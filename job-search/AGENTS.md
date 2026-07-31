# job-search — agent rules

Read [`/AGENTS.md`](../AGENTS.md) first. This adds rules specific to `job-search/`.

## What this owns

Saved jobs, bookmarks, search criteria. Its own database.

## Hard rules

- **Do not scrape job boards.** API-based discovery and manual user input only. Scraping is legally grey, technically fragile, and the same risk class as the LinkedIn constraint in [ADR 0003](../docs/decisions/0003-input-sources.md). If an API doesn't cover something, the manual path covers it.
- **The manual path is a first-class feature**, not a fallback. It ships with the API path, not after it.
- **`jobId` is UUIDv4, minted by us.** External APIs have their own IDs — store them as a separate field, never expose one as `jobId`.
- **Emit activity events** for saves and bookmarks, with a client-generated `eventId`.

## External APIs

Adding or changing a job API provider is a team decision — cost, ToS, rate limits, and data handling all apply. See [`/.agents/boundaries.md`](../.agents/boundaries.md).

Handle rate limits and outages gracefully. An external API being down must not break the manual path.

## Not decided yet

- Which external API provider — open, and it affects the result shape
- Whether a JD lives here or in `jd-analysis/` once analyzed. A job can arrive by search or by paste; settle the ownership with that module's owner before building either side.
- When a job becomes persistent (on view? on save?) and who mints `jobId` in the manual-paste path — see [`open-questions.md`](../docs/open-questions.md) item 5.
