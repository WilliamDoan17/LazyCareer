# 0006 — Auth & UI Architecture

## Decision
**One unified frontend web app**, not one UI per module. Single login, single JWT, attached to every API call to every backend service. Each backend service still verifies the JWT independently (per the spec's Phase 0 "single auth provider, verified by every service independently").

## Why
Backends stay decoupled microservices (independent DBs, independent deploys) — it's only the frontend that's unified. This front-loads what the project spec calls the Phase 3 "Pipeline UI shell" into Phase 1: build the shell now, add a route/section per module as it ships, instead of stitching together separately-built module UIs later during integration.

## Consequence
- Auth provider (e.g. Supabase Auth or Clerk) is configured once, in Phase 0, and used by the frontend and every backend service.
- The frontend is the one shared surface across the team; backends remain independently ownable per module.
