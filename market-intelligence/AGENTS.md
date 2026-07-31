# market-intelligence — agent rules

Read [`/AGENTS.md`](../AGENTS.md) first. This adds rules specific to `market-intelligence/`.

**Phase 4, V2 stretch. Do not start work here** unless explicitly asked.

## What this owns

Market data and derived strategy output. The profile stays in `career-memory`.

## Hard rules

- **Read the profile over REST.** Never query `career-memory`'s database, never keep a copy.
- **No scraping.** Same constraint as `job-search/` and the LinkedIn rule in [ADR 0003](../docs/decisions/0003-input-sources.md) — licensed APIs or nothing. Market data sites are aggressive about this.
- **Attribute market claims to a source.** "Demand for X is rising" with nothing behind it is a guess presented as intelligence, and users will make career decisions on it.
- **Adding an external data source is a team decision** — cost, ToS, licensing. See [`/.agents/boundaries.md`](../.agents/boundaries.md).

## Not decided yet

**The data source itself is unidentified.** That's the blocking question for this module — everything else is downstream of it. Nothing here should be designed until it's answered.
