# Boundaries

What to **stop and ask about** rather than decide alone.

[`rules.md`](rules.md) says what you must not do. This says where your authority ends — actions that may be perfectly correct but are not yours to take unilaterally, because they commit four people to something.

## Stop and ask

### Contracts
Changing anything another module consumes: an endpoint's shape, a field's type, a status code's meaning, an event's payload, a shared type in `shared/`.

Additive and backwards-compatible? Proceed. Anything that could break a consumer is a conversation before it is a commit.

### Cross-module dependencies
Making module A depend on module B where it didn't before. Dependencies are recorded in [`architecture.md`](../docs/architecture.md) and each module's `README.md` — a new one changes the build order and the team split.

### New services
Adding a folder to the nine in [ADR 0007](../docs/decisions/0007-repo-structure.md). The module list comes from the product spec; a tenth service is a product decision, not an implementation detail.

### Stack choices
Introducing a language, framework, package manager, ORM, database, queue, or build tool. **The stack is explicitly undecided** — see [`open-questions.md`](../docs/open-questions.md). Picking one by writing a config file decides it by accident for everyone.

This includes the small ones. A test runner added "just for this module" is a stack decision.

### External services and dependencies
Adding a third-party API, SaaS, or non-trivial package. Cost, ToS, data handling, and lock-in are team concerns.

### Anything touching auth or secrets
Auth flow changes, token handling, encryption of stored credentials (`github_connections.access_token` is encrypted at rest for a reason), or anything that moves a secret.

### Reversing a decision
If your work requires contradicting an ADR, the ADR may well be wrong — but say so and write a superseding ADR. Don't quietly build against it.

## Folder ownership

**Do not edit another module's folder.** Each service is owned by a person (`Owner: TBD` for now, filled in from the team split).

Need something from another service? Call its REST API. Need a shared shape? Put it in `shared/` — and note that a `shared/` change affects everyone, so flag it.

## Scope

Do what was asked. Adjacent problems get **reported, not fixed** — in a repo with no code and no tests, an unrequested "while I was in there" change has nothing to catch it if it's wrong.

Specifically: don't refactor code you were asked to extend, don't reformat files you were asked to edit, and don't fix a bug you found in another module — report it.

## Safe without asking

- Writing code inside the module you were asked to work on
- Adding documentation, or fixing docs that are wrong or stale
- Adding tests
- Additive, backwards-compatible API changes
- Reporting a problem you found anywhere

## When you're unsure

Ask. Phase 0 exists precisely because guessing wrong about contracts turns a two-week Phase 3 into a two-month refactor. That's the whole thesis of the project plan — an unnecessary question costs a minute, a wrong assumption costs weeks.

---

Next: [`workflows.md`](workflows.md) — how to do the common things.
