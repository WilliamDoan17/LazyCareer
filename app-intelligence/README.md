# app-intelligence

**Module 7 — Application Intelligence & Skill Gap Analyzer.** Turns accumulated application history into self-generated feedback and a skill development roadmap.

| | |
|---|---|
| **Kind** | REST API / microservice |
| **Phase** | 4 — **V2 stretch** |
| **Complexity** | Medium |
| **Status** | Empty — not started, and not startable yet |
| **Owner** | TBD |
| **Depends on** | application-manager (tracker history), career-memory, jd-analysis |
| **Depended on by** | pipeline |

## Responsibility

Solves problem 5: *no self-generated feedback loop*.

- Response rate analytics by role type, source, seniority, company size
- Fit score history across all applications
- Aggregate skill gap: skills most frequently required but missing from the profile
- Resume quality score before submission
- Pattern summary: what's not working and why

## Owns

Derived analytics only. Every input belongs to another service.

## Why Phase 4

Requires **accumulated real user data** from Phases 1–3. Response rate analytics over five applications is noise. Building this early produces a module that technically works and says nothing.

## Status

Not startable. Depends on tracker history that does not exist yet.

See [`AGENTS.md`](AGENTS.md) · [architecture](../docs/architecture.md)
