# LazyCareer Docs

The human-facing documentation tree. Agent-facing rules live in [`../.agents/`](../.agents/); product-facing description lives in the [root README](../README.md).

**New here? → [START-HERE.md](START-HERE.md)** (5 minutes)

## Pages

| Page | What it's for |
|---|---|
| [START-HERE.md](START-HERE.md) | Onboarding route. Read first. |
| [architecture.md](architecture.md) | How the system fits together — diagram, service boundaries, data flows, auth |
| [glossary.md](glossary.md) | What our terms mean |
| [conventions.md](conventions.md) | Branches, commits, PRs, ADR process, env config |
| [open-questions.md](open-questions.md) | What's still undecided, and what each thing blocks |

## Folders

- [`decisions/`](decisions/) — architecture decision records (ADRs), one per decision, numbered in the order they were made. **Authoritative.**
- [`schema/`](schema/) — DB schema docs per service
- [`spec/`](spec/) — original product spec. **Historical** — ADRs override it where they conflict.

## Decisions

1. [Career Memory product form](decisions/0001-career-memory-product-form.md)
2. [Storage model](decisions/0002-storage-model.md)
3. [Input sources](decisions/0003-input-sources.md)
4. [ID format](decisions/0004-id-format.md)
5. [API conventions (REST, versioning, errors)](decisions/0005-api-conventions.md)
6. [Auth & UI architecture](decisions/0006-auth-and-ui-architecture.md)
7. [Repo structure](decisions/0007-repo-structure.md)

## Schema

- [Career Memory schema](schema/career-memory.md)

## Spec

- [Project Specification v2](spec/project-spec-v2.md) — converted to markdown; `.docx` archived alongside it

---

**Adding a doc?** Add it to the table above in the same PR. An unlisted doc is an undiscoverable doc.
