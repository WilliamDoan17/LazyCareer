# career-memory

**Module 1 — Automated Career Memory.** A living, structured profile of the user that updates as they use the app. The data foundation everything else depends on.

| | |
|---|---|
| **Kind** | REST API / microservice + thin UI ([ADR 0001](../docs/decisions/0001-career-memory-product-form.md)) |
| **Phase** | 1 |
| **Complexity** | Medium |
| **Status** | Empty — not started |
| **Owner** | TBD |
| **Depends on** | Nothing — build it first |
| **Depended on by** | jd-analysis, document-tailor, app-intelligence, market-intelligence, pipeline |

## Responsibility

Owns who the user is. Solves problem 1: *no single source of truth for experience & activity*.

- Onboarding intake: resume, LinkedIn URL, GitHub, chat, or manual entry
- Structured profile storage: skills, experience, projects, education, certifications
- Append-only activity log fed passively by every other module
- Manual add / edit
- Export profile as Markdown or JSON — **generated on request, never authoritative**

## Data

**[Schema: `docs/schema/career-memory.md`](../docs/schema/career-memory.md) — required reading before touching anything here.**

Postgres, relational. Key points that aren't obvious from table names:
- `experience_skills` / `project_skills` link a skill to the *specific role or project* where it was used — fit scoring depends on that evidence chain
- `activity_log` is append-only and deduped on `eventId`
- `skills` has `unique(user_id, name)` — upsert, don't insert

## Intake sources

Five, with different refresh behaviour ([ADR 0003](../docs/decisions/0003-input-sources.md)):

| Source | Refresh |
|---|---|
| Chat | on user message |
| Resume files | on upload |
| Logging files | on upload |
| GitHub | live — webhooks + polling |
| LinkedIn | **manual only** — never on a timer |

## Status

Blocked on the [tech stack decision](../docs/open-questions.md). This is the first module to build once it lands.

See [`AGENTS.md`](AGENTS.md) · [architecture](../docs/architecture.md)
