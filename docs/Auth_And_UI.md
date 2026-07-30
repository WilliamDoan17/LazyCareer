# Auth & UI Architecture

## One unified frontend
A single frontend web app, not one UI per module. Single login, single JWT, attached to every API call to every backend service. Each backend service still verifies the JWT independently (single auth provider — e.g. Supabase Auth or Clerk — configured once in Phase 0, used everywhere).

Backends stay decoupled microservices (independent DBs, independent deploys); only the frontend is unified. This front-loads what the project spec calls the Phase 3 "Pipeline UI shell" into Phase 1: build the shell now, add a route/section per module as it ships, instead of stitching together separately-built module UIs later during integration.

## Consequence
- Auth provider is configured once, in Phase 0, and used by the frontend and every backend service.
- The frontend is the one shared surface across the team; backends remain independently ownable per module.
- The `shared/` package (types, schema validators, auth helpers, UI kit) is the only place agreed data shapes and auth logic live — every service imports from it, never duplicates it.
