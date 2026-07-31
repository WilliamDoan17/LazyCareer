# job-search

**Module 3 — Job Search & Filter.** Find roles matching the user's criteria without leaving the app.

| | |
|---|---|
| **Kind** | REST API / microservice |
| **Phase** | 1 — leads in priority within Phase 1 |
| **Complexity** | Medium–High |
| **Status** | Empty — not started |
| **Owner** | TBD |
| **Depends on** | External job APIs (Adzuna, JSearch, or similar) |
| **Depended on by** | jd-analysis, pipeline |

## Responsibility

Solves problem 3: *no structured process for finding roles that match me*.

- Filter by role, location, experience level, salary, company size, remote/hybrid
- API-based results for discovery
- **Manual input**: paste a URL or fill a job entry form — fallback and power-user path
- Each result links directly into JD Analysis
- Save and bookmark roles

## Owns

Saved and bookmarked jobs, search criteria. Its own database.

## The dual approach

API **and** manual input, both from day one. Scraping job boards is legally grey and technically fragile, and API coverage will always have gaps — the manual path makes the module useful regardless. It is not a fallback to bolt on later.

## Status

Blocked on the [tech stack decision](../docs/open-questions.md). The external job API choice (Adzuna vs JSearch vs other) also needs deciding — cost, coverage, and rate limits.

See [`AGENTS.md`](AGENTS.md) · [architecture](../docs/architecture.md)
