# Build Plan / Backlog

Ordered work items, thinnest-slice-first. Phase 0 items are waterfall (get right once, before anything else). Everything after is Agile — build the smallest working slice, run it, then iterate; don't complete a module's full feature/schema surface before testing it.

## Phase 0 — Infra prerequisites (do once, in order)
- [ ] Scaffold monorepo folders + workspace config (npm/pnpm workspaces) — [Repo_Structure.md](Repo_Structure.md)
- [ ] Create auth provider account (Supabase or Clerk), get JWKS URL — [Auth_And_UI.md](Auth_And_UI.md)
- [ ] Bootstrap `shared/` package skeleton: JWT-verify helper, UI kit stub, empty types folder
- [ ] Docker Compose skeleton (even with just one service listed to start)
- [ ] `.env.example` per service per [Env_Config.md](Env_Config.md)

## Frontend shell
- [ ] Scaffold frontend app (routing, base layout)
- [ ] Login page wired to the real auth provider
- [ ] Empty dashboard route (placeholder — real dashboard comes with Pipeline, Phase 3)

## Career Memory — thinnest vertical slice
- [ ] Postgres instance + `DATABASE_URL` wired up
- [ ] Migration: `users` + `skills` tables only (rest of schema comes later, per [Career_Memory.md](Career_Memory.md))
- [ ] `POST /api/v1/users/:userId/skills` — create
- [ ] `GET /api/v1/users/:userId/skills` — list
- [ ] JWT verification middleware using `shared/`'s auth helper
- [ ] Frontend: minimal "add skill" form calling the live API
- [ ] **End-to-end check**: log in → add a skill via the UI → confirm it's in Postgres → fetch and see it rendered

Nothing past this point gets built until the slice above actually runs.

## Next slices (backlog, not yet started — reprioritize after the first slice works)
- [ ] `experience` table + endpoints
- [ ] `projects`, `education`, `certifications` tables + endpoints
- [ ] `experience_skills` / `project_skills` join tables
- [ ] Resume upload + parsing (PDF/DOCX/TeX → structured entries)
- [ ] Chat-driven intake interface (replaces/augments the manual form)
- [ ] GitHub OAuth connect + first-import sync
- [ ] GitHub live sync (webhook or polling)
- [ ] LinkedIn URL paste import + manual "Update" button
- [ ] `activity_log` table + `POST /activity` endpoint (per [Event_Schema.md](Event_Schema.md))
- [ ] Markdown/JSON export endpoint

## After Career Memory has a working slice
- Start Job Search & JD Analysis (Phase 1, built in parallel per the spec) — each gets its own thinnest-slice backlog when picked up.
