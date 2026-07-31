# pipeline

**Module 6 — Unified Application Pipeline.** The orchestration layer that connects every module into one continuous flow.

| | |
|---|---|
| **Kind** | REST API / microservice — orchestration layer |
| **Phase** | 3 |
| **Complexity** | Medium |
| **Status** | Empty — not started |
| **Owner** | TBD — needs someone with full system context |
| **Depends on** | Every module |
| **Depended on by** | frontend |

## Responsibility

Solves problem 7: *no pipeline for fast, end-to-end application*. This is the phase that turns a collection of tools into a product.

- Single entry point: search or paste a job to begin
- Linear flow: **Search → JD Analysis → Procedure Guide → Tracker → Document Tailor**
- Passive Career Memory updates at every step
- Dashboard: active applications, upcoming deadlines, recent activity, fit scores at a glance
- State continuity: `{ userId, jobId, applicationId }` persists across modules

## The rule that defines this module

**It calls module APIs. It reimplements nothing.**

Every piece of logic already lives in a module. If the pipeline starts computing a fit score or formatting a resume, the architecture has failed. It orchestrates and it holds cross-module context — that's all.

## Owns

Cross-module session context. No domain data of its own.

## Status

Phase 3 — starts once Phases 1 and 2 have shipped. The [shared context shape](../docs/open-questions.md) it depends on is still undecided.

See [`AGENTS.md`](AGENTS.md) · [architecture](../docs/architecture.md)
