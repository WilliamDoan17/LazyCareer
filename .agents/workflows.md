# Workflows

How to do the recurring things correctly.

## Commands

**TODO — pending tech stack decision.** There is nothing to install, build, run, or test yet. See [`open-questions.md`](../docs/open-questions.md) item 1.

Do not invent commands. If asked to run something and no command exists, say so.

## Record a decision

1. Next number in [`docs/decisions/`](../docs/decisions/) — `NNNN-short-title.md`
2. Three sections: **Decision** → **Why** → **Consequence**. Short. Existing ADRs are 10–25 lines; match that.
3. Add it to the index in [`docs/README.md`](../docs/README.md)
4. If it closes an item in [`open-questions.md`](../docs/open-questions.md), move that entry to **Resolved** with a link
5. If it supersedes an earlier ADR, say so in the new one **and** add a supersession note to the old one — but never otherwise edit a merged ADR

## Answer "why is it like this?"

Search [`docs/decisions/`](../docs/decisions/) first. Seven ADRs cover product form, storage, intake, IDs, API conventions, auth/UI, and repo structure — most "why" questions are already answered there.

Not in an ADR? Check [`open-questions.md`](../docs/open-questions.md) — it may be undecided rather than undocumented. Those are very different answers.

## Add a module

Don't — see [`boundaries.md`](boundaries.md). The nine folders come from [ADR 0007](../docs/decisions/0007-repo-structure.md) and the product spec.

Adding files *inside* an existing module folder is normal work.

## Commit

Conventional Commits, imperative mood, folder as scope:

```
feat(career-memory): add skills CRUD endpoints
fix(application-manager): dedupe activity events on eventId
docs: record event bus decision as ADR 0008
```

**One logical change per commit**, each leaving the repo coherent. Body explains *why*; the diff already shows *how*. Full detail in [`conventions.md`](../docs/conventions.md).

## Add an endpoint

1. `/api/v1/...` path, REST/JSON
2. Errors use the shared envelope from `shared/` — never a local error shape
3. Semantic status codes
4. UUIDv4 for any ID in the response
5. Event-driven write? Require `eventId` and dedupe on it
6. Document it in the module's `README.md`

All binding — see [`rules.md`](rules.md).

## Touch Career Memory data

Read [`docs/schema/career-memory.md`](../docs/schema/career-memory.md) first. Non-optional — the schema encodes decisions that are not obvious from the table names:

- `experience_skills` / `project_skills` exist so a skill links to the *specific role or project* where it was used, not just globally. Fit scoring depends on that evidence chain.
- `bullets` is JSONB deliberately — no module queries into individual bullets.
- `activity_log` is append-only and deduped on `eventId`.
- `unique(user_id, name)` on `skills` prevents duplicates from repeated resume/GitHub parses. Upsert, don't insert.

## Consume data from another service

Call its REST API with the user's JWT. Never query its database. Never cache its data as if you owned it.

Service-to-service auth mechanics are **not yet decided** — [`open-questions.md`](../docs/open-questions.md) item 6. If your task requires it, flag that rather than inventing a scheme.

## Update documentation

| Change | Goes in |
|---|---|
| Product-facing | [`README.md`](../README.md) |
| How the system works | [`docs/architecture.md`](../docs/architecture.md) |
| Why we chose something | a new ADR |
| A new term | [`docs/glossary.md`](../docs/glossary.md) |
| A rule agents must follow | [`rules.md`](rules.md) |
| Something now decided | remove from [`open-questions.md`](../docs/open-questions.md) |

**One fact, one home.** Restating rather than linking is how docs drift, and drifted docs are worse than none.
