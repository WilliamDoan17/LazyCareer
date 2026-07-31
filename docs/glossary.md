# Glossary

Terms that mean something specific here. If a term appears in code, an API, or a PR, this is what it means.

## Product concepts

**Career Memory** — Module 1, and the product's foundation. A living structured profile of the user (skills, experience, projects, education, certifications) that updates automatically as they use the app. Also the name of the service that owns it. When someone says "the profile," they mean Career Memory data.

**Activity log** — the append-only, event-sourced record of everything the user has done, inside LazyCareer or out (`status_change`, `procedure_step_completed`, `github_commit`, …). Entries are never mutated or deleted. Lives in `career-memory`.

**Passive update** — a Career Memory change the user did not explicitly make. Another module emits an activity event as a side effect of normal use, and Career Memory appends it. This is what "automated" in "Automated Career Memory" refers to.

**Intake / input source** — one of the five ways profile data enters Career Memory: chat, resume files, logging files, GitHub, LinkedIn. Each has its own refresh behaviour — see [architecture.md](architecture.md#intake).

**Logging file** — a document (PDF/DOCX/TeX) describing a project or career progress, uploaded to be parsed and appended to the profile. Distinct from a **resume file**, which describes overall history.

**Fit score** — a numeric measure of how well a user's Career Memory profile matches a specific job description. Produced by `jd-analysis`. Evidence-based: backed by the specific experience or project where a skill was actually used, not a flat skill-name match.

**Gap summary** — the output of comparing a JD's requirements against the profile: what the user has vs. what the role wants.

**Procedure guide** — Module 4a. The steps required to apply for a given role type (SWE, PM, design…), as a checklist. Answers *what do I need to do*.

**Progress tracker** — Module 4b. Where each application currently stands. Answers *where am I in doing it*. Ships together with the procedure guide as **Application Manager**, because either alone is useless.

**Application stage** — where an application sits in the funnel. The canonical ordered set:

```
Applied → OA → Phone Screen → Interview → Offer → Rejected/Ghosted
```

`OA` = online assessment. `Rejected/Ghosted` is terminal and covers both explicit rejection and silence.

**Pipeline** — Module 6 / Phase 3. The orchestration layer and UX wrapper that connects the modules into one continuous flow. **Not** a CI/CD pipeline. When ambiguity is possible, say "the application pipeline" or "CI".

**Shared context** — `{ userId, jobId, applicationId }`, the identifiers that follow a user across modules so the flow feels continuous. Owner and exact shape are still open — see [open-questions.md](open-questions.md).

## Structural terms

These three get used interchangeably in conversation and should not be:

**Module** — a unit of *product* scope. There are 8, numbered, each solving a numbered problem. "Module 4" is Application Manager.

**Service** — a unit of *deployment*. One backend, one database, one folder, deployed independently. Modules mostly map 1:1 to services, but not always: Module 4 (4a + 4b) is one service, and `frontend/` and `shared/` are folders that are not modules at all.

**Phase** — a unit of *time*. What ships together. Phases 0–4. A phase contains multiple modules built in parallel.

**`shared/`** — the package holding types, schema validators, auth helpers, and the UI kit. The *only* place agreed data shapes live. Every service imports from it; nothing duplicates it.

## Technical terms

**ADR** — Architecture Decision Record. One numbered file per decision in [`decisions/`](decisions/), recording what was decided and why. Immutable once merged: superseding an ADR means writing a new one, never editing the old one.

**Error envelope** — the single response shape every service returns on failure. Defined once in `shared/`. See [ADR 0005](decisions/0005-api-conventions.md).

**`eventId`** — a client-generated UUID attached to every event-driven write (`POST /activity`, webhook receivers). The receiver dedupes on it. Exists because retries and duplicate deliveries are normal, not exceptional.

**Idempotent** — safe to receive more than once. A duplicate delivery produces no additional effect. Enforced here by `eventId` plus a unique index.

**Database per service** — each service owns its tables exclusively. No shared tables, no cross-service joins, no reading another service's database. Cross-service data access happens over REST, always.

**Source of truth** — the authoritative store for a piece of data. For profile data it is Career Memory's Postgres tables. Markdown/JSON exports are *derived output* generated on request, never authoritative.

## Status labels

**Core** — required for MVP. Modules 1–6.

**V2 Stretch** — deferred to Phase 4. Modules 7–8. Meaningful only once real user data has accumulated.

**TBD** — genuinely undecided and tracked in [open-questions.md](open-questions.md). Not a placeholder someone forgot to fill in.
