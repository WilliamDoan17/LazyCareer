# shared

Types, schema validators, auth helpers, and the UI kit. The **only** place agreed data shapes live.

| | |
|---|---|
| **Kind** | Library — not a service, not deployed |
| **Phase** | 0 |
| **Status** | Empty — not started |
| **Owner** | TBD |
| **Depends on** | Nothing in this repo |
| **Depended on by** | Every service and the frontend |

## Responsibility

Phase 0's central output. Every module that reads or writes user experience/activity data uses shapes defined here, so the Phase 3 integration is a connect job rather than a refactor.

Holds:
- **Types** for cross-service data shapes
- **Schema validators** for those types
- **The error envelope** — `{ error: { code, message, details } }`, defined once here
- **Auth helpers** for JWT verification, used identically by every service
- **UI kit** consumed by `frontend/`

## Rules

- If two services need the same shape, it belongs here and both import it. Copy-pasting a type across services is the exact failure Phase 0 exists to prevent.
- `shared/` depends on nothing else in this repo. If it needs a service, the design is wrong.
- A change here affects everyone — flag it explicitly in the PR.

## Status

Cannot start until the tech stack is decided ([open questions](../docs/open-questions.md) item 1) — the package format depends on it.

See [`AGENTS.md`](AGENTS.md) · [ADR 0005](../docs/decisions/0005-api-conventions.md) · [ADR 0006](../docs/decisions/0006-auth-and-ui-architecture.md)
