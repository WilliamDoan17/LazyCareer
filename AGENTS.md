# AGENTS.md

Agent entry point for **LazyCareer** — a modular job-search and application platform. Monorepo, microservices, 4-person team.

Read this file fully before acting. It is enough to work without breaking anything.

## State: Phase 0

**No code exists yet.** Contracts are being agreed. The **tech stack is undecided** — do not introduce a language, framework, package manager, ORM, or build tool. There is nothing to install, build, or run; don't invent commands.

## The non-negotiables

Violating one of these breaks another team member's module. All are decided and binding.

1. **ADRs beat the spec.** [`docs/decisions/`](docs/decisions/) is authoritative. [`docs/spec/`](docs/spec/) is historical ideation and conflicts with the ADRs in four known places — those are resolved in [`.agents/rules.md`](.agents/rules.md).
2. **UUIDv4** for every ID crossing a service boundary. Never auto-increment.
3. **REST/JSON only**, `/api/v1/...` path versioning. No GraphQL, no tRPC.
4. **One error envelope**, imported from `shared/`, never redefined: `{ error: { code, message, details } }`.
5. **Every event-driven write carries a client-generated `eventId`** and is deduped server-side. Duplicate deliveries are normal.
6. **Database per service.** No shared tables, no cross-service joins, no reading another service's database. REST only.
7. **Every service verifies the JWT independently.** Never trust an upstream caller to have checked.
8. **LinkedIn is manual-sync only.** Never poll it, never scrape it in the background — ToS and account-ban risk.
9. **Shared types, validators, and auth helpers live in `shared/` only.** Never duplicate a shape across services.
10. **Ask before changing a contract, adding a cross-module dependency, or picking any part of the stack.**

## Where to go

| You need | Read |
|---|---|
| The full rules, with rationale links | [`.agents/rules.md`](.agents/rules.md) |
| What requires asking first | [`.agents/boundaries.md`](.agents/boundaries.md) |
| How to do a common task | [`.agents/workflows.md`](.agents/workflows.md) |
| How the system fits together | [`docs/architecture.md`](docs/architecture.md) |
| What a term means | [`docs/glossary.md`](docs/glossary.md) |
| Why something is the way it is | [`docs/decisions/`](docs/decisions/) |
| What's still undecided | [`docs/open-questions.md`](docs/open-questions.md) |
| Career Memory data — **required before touching it** | [`docs/schema/career-memory.md`](docs/schema/career-memory.md) |

Working inside a module folder? It has its own `AGENTS.md` scoped to that service. Read it too.

## Repo map

```
shared/               types, validators, auth helpers, UI kit — the only home for shared shapes
frontend/             the one unified web app
career-memory/        Module 1 — living profile + activity log. Everything depends on it.
job-search/           Module 3 — job discovery
jd-analysis/          Module 2 — JD breakdown, fit score, gap summary
application-manager/  Module 4 — procedure guide + progress tracker
document-tailor/      Module 5 — resume / cover letter / post generation
pipeline/             Module 6 — Phase 3 orchestration
app-intelligence/     Module 7 — Phase 4 stretch
market-intelligence/  Module 8 — Phase 4 stretch

docs/                 human-facing documentation
.agents/              agent-facing rules
```

## Working here

- **Commit small**, Conventional Commits with the folder as scope: `feat(career-memory): add skills CRUD endpoints`. One logical change per commit.
- **Stay in your module.** Cross-service needs go through REST or `shared/`.
- **Do what was asked.** Report adjacent problems; don't fix them unasked.
- **One fact, one home.** Documenting something? Link to the existing source, never restate it.
- **When unsure, ask.** Phase 0 exists because wrong assumptions about contracts cost weeks later.
