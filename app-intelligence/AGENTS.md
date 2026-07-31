# app-intelligence — agent rules

Read [`/AGENTS.md`](../AGENTS.md) first. This adds rules specific to `app-intelligence/`.

**Phase 4, V2 stretch. Do not start work here** unless explicitly asked — Phases 1–3 come first, and this module is meaningless without their accumulated data.

## What this owns

Derived analytics. Nothing else — every input is read from another service.

## Hard rules

- **Read-only across services.** Pull history from `application-manager`, profile from `career-memory`, scores from `jd-analysis`, all over REST. Never query their databases. Never write back to them.
- **Derived data is disposable.** Anything computed here must be recomputable from the source services. Never let an analytic become a source of truth.
- **Report honestly on thin data.** With few applications, the correct output is "not enough data yet," not a confident-looking number. A misleading response-rate stat is worse than none.

## Not decided yet

Everything downstream of the Phase 1–3 stack decisions. Nothing here should be designed until the modules it reads from actually exist and their APIs are stable.
