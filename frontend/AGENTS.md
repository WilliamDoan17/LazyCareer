# frontend — agent rules

Read [`/AGENTS.md`](../AGENTS.md) first. This adds rules specific to `frontend/`.

## What this owns

UI state. Nothing else. Every piece of domain data belongs to a backend service and is fetched from it.

## Hard rules

- **One app, one login, one JWT.** Attach it to every request to every service. Don't build a per-module auth path.
- **Never cache service data as if you own it.** If Career Memory owns the profile, the frontend renders it — it doesn't become a second source of truth.
- **REST only.** No direct database access, ever, and no assuming two services can be joined server-side — they can't.
- **Shared components go in `shared/`'s UI kit** once a second section needs them. Don't fork a component per module.

## Adding a module section

A new section is a route plus API calls to that module's existing REST endpoints. If you find yourself reimplementing module logic in the frontend, stop — that logic belongs in the service.

## Not decided yet

Framework, build tooling, and the auth provider are all open — see [`open-questions.md`](../docs/open-questions.md). Don't pick one by scaffolding an app.
