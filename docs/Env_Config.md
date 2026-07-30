# Environment & Config Conventions

## Convention
One `.env` per service folder (not a global `.env`) — Docker Compose isolates each service's environment, so names don't need a per-service prefix. Each service commits a `.env.example` with the same keys (no real values) so the required shape is documented without leaking secrets.

## Career Memory — needed now

**Data + storage**
- `DATABASE_URL` — Postgres connection string
- `BLOB_STORAGE_BUCKET`, `BLOB_STORAGE_ACCESS_KEY`, `BLOB_STORAGE_SECRET_KEY` — resume/log file uploads (S3-compatible; local dev can use MinIO)

**Auth**
- `AUTH_JWKS_URL` — wherever the shared auth provider (Supabase/Clerk) publishes its keys, so this service verifies JWTs independently. No auth secret needed beyond this if verification is signature-based against the public JWKS endpoint.

**LLM** (powers chat-driven intake + resume/log-file parsing)
- `ANTHROPIC_API_KEY` (or equivalent provider key)

**GitHub live sync**
- `GITHUB_CLIENT_ID`, `GITHUB_CLIENT_SECRET` — OAuth app credentials
- `GITHUB_WEBHOOK_SECRET` — verifies incoming webhook payloads

**Service basics**
- `PORT`
- `NODE_ENV`

## Explicitly not needed yet
- LinkedIn — no API key required; it's a manual URL paste/fetch triggered by the user, not an authenticated API integration.
- Any other module's env vars (Job Search's Adzuna/JSearch keys, etc.) — those get defined in that service's own `.env` when that service is actually built, not here.
