# frontend

The one unified web app. Single login, one route/section per module.

| | |
|---|---|
| **Kind** | Web app |
| **Phase** | 1 onwards — the shell is built early, modules are added as they ship |
| **Status** | Empty — not started |
| **Owner** | TBD |
| **Depends on** | `shared/` (UI kit, types), every backend service |
| **Depended on by** | Nothing |

## Responsibility

The single user-facing surface. Not one UI per module — [ADR 0006](../docs/decisions/0006-auth-and-ui-architecture.md) decided one app with a section per module, so the Phase 3 "pipeline UI shell" is built up front instead of stitched together during integration.

- Handles login against the shared auth provider
- Attaches the JWT to every API call to every service
- Owns UI state only — never a copy of a service's data

## Rules

- Backends stay decoupled; only the frontend is unified. Don't let UI convenience drive a backend coupling.
- Every service is reached over its REST API. The frontend never assumes two services share a database, because they don't.
- Shared components live in `shared/`'s UI kit, not here, once more than one section needs them.

## Status

Blocked on the [tech stack](../docs/open-questions.md) and [auth provider](../docs/open-questions.md) decisions.

See [`AGENTS.md`](AGENTS.md) · [architecture](../docs/architecture.md)
