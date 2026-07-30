# Repo Structure

## Monorepo, not submodules
Single monorepo with workspaces (npm/pnpm/yarn workspaces, optionally Turborepo/Nx).

```
lazycareer/
  shared/            — types, schema validators, auth helpers, UI kit
  frontend/          — the unified web app
  career-memory/
  job-search/
  jd-analysis/
  application-manager/
  document-tailor/
  pipeline/          — Phase 3 integration layer
  app-intelligence/  — Phase 4
  market-intelligence/ — Phase 4
```

Each folder still builds and deploys independently (own Dockerfile/CI job) — the monorepo unifies source control, not deployment.

## Why not submodules
Submodules solve a problem this team doesn't have: genuinely separate projects with different owners/release cadences/access control. They cost constantly instead — stale commit pointers, detached-HEAD confusion, extra CI steps just to check out submodules correctly. A monorepo gets the same independent deployability while making cross-cutting changes (e.g. updating `shared/` types and the services that consume them) a single atomic commit instead of a multi-repo PR dance. `shared/` and the unified `frontend/` need tight coupling — keeping them in separate repos would recreate the submodule problem by hand.

## Deployment
Each service deployed independently to Railway or Render (not Kubernetes at this stage). Local development via Docker Compose so every service runs together in one command.
