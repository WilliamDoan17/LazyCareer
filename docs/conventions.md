# Conventions

How we work in this repo. Short by design — if a rule isn't here, use judgement and match what's already there.

> Stack-specific tooling (linter, formatter, test runner, package manager) is
> **not decided yet** — see [open-questions.md](open-questions.md). Sections
> marked TODO fill in with that decision.

## Branches

```
<type>/<short-description>
```

Types match commit types below. Examples:

```
feat/career-memory-skills-endpoint
fix/activity-log-duplicate-events
docs/adr-event-bus
chore/upgrade-deps
```

Branch off `main`. Keep branches short-lived — a branch that lives a week in a 4-person monorepo is a merge conflict waiting to happen.

## Commits

Conventional Commits. Subject line in imperative mood, no trailing period, ≤72 chars.

```
<type>(<scope>): <what this change does>

<why it was needed — the context a reviewer or future reader lacks>
```

**Types:** `feat` `fix` `docs` `refactor` `test` `chore` `perf`

**Scope** is the folder: `career-memory`, `frontend`, `shared`, … Omit it for repo-wide changes.

```
feat(career-memory): add skills CRUD endpoints
fix(application-manager): dedupe activity events on eventId
docs: record event bus decision as ADR 0008
```

**Commit small.** One logical change per commit, each leaving the repo coherent. The commit log is the project's traceability — a reviewer should be able to read `git log --oneline` and understand what happened without opening a diff.

## Pull requests

- One module per PR wherever possible. A PR touching three services needs a reason.
- Changes to `shared/` affect everyone — flag them explicitly in the PR description and expect closer review.
- PR description covers **what** and **why**. The diff already shows how.
- Link the ADR if the change implements a decision. Write the ADR first if it makes one.
- Squash-merge unless the individual commits carry real history worth keeping.

## Decisions

Any choice a future teammate could reasonably question gets an ADR in [`decisions/`](decisions/).

- One decision per file, numbered sequentially: `NNNN-short-title.md`
- Structure: **Decision** → **Why** → **Consequence**
- **Never edit a merged ADR.** Superseding one means writing a new ADR that says so, and adding a supersession note to the old one. The record of what we believed and when is the point.
- Add the new entry to the [docs index](README.md)

## Code layout

Every service folder is self-contained:

```
<service>/
  README.md      — what it does, status, dependencies
  AGENTS.md      — rules scoped to this service
  ...            — source, tests, Dockerfile, CI config
```

Do not reach into another service's folder. Cross-service needs go through its REST API, or through `shared/` if it's a type or helper.

## Shared code

`shared/` holds types, schema validators, auth helpers, and the UI kit — and nothing else.

- If two services need the same shape, it belongs in `shared/`. Copy-pasting a type across services is the failure mode Phase 0 exists to prevent.
- `shared/` depends on nothing in this repo. If it needs a service, the design is wrong.

## Environment & config

- Config comes from environment variables. No secrets in code, no secrets in git.
- Every service ships a committed `.env.example` listing every variable it reads, with safe placeholder values. It is the documentation of what the service needs.
- Real `.env` files are gitignored and never committed.
- Variable naming: `SCREAMING_SNAKE_CASE`, prefixed by concern — `DATABASE_URL`, `AUTH_JWT_ISSUER`, `GITHUB_CLIENT_SECRET`.
- Anything a service cannot run without should fail loudly at startup, not silently at first use.

## API surface

Binding rules live in [`.agents/rules.md`](../.agents/rules.md) and [ADR 0005](decisions/0005-api-conventions.md). In short: REST/JSON, `/api/v1/...` path versioning, the shared error envelope, semantic status codes, `eventId` on event-driven writes.

Additive changes don't need a version bump. Breaking changes do — and breaking a contract another module consumes is a conversation before it is a commit.

## Testing

**TODO — pending tech stack decision.** Test runner, coverage expectations, and CI gates get defined with the stack.

The one rule that holds regardless: contract behaviour other modules depend on gets a test.

## Documentation

- Product-facing changes → [`README.md`](../README.md)
- How the system works → [`architecture.md`](architecture.md)
- Why we chose something → a new ADR
- New domain term → [`glossary.md`](glossary.md)
- A rule an agent must follow → [`.agents/`](../.agents/)

**One fact, one home.** If you find yourself restating something documented elsewhere, link to it instead. Duplicated documentation drifts, and drifted documentation is worse than none.
