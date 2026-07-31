# career-memory — agent rules

Read [`/AGENTS.md`](../AGENTS.md) first. This adds rules specific to `career-memory/`.

**Before touching data here, read [`docs/schema/career-memory.md`](../docs/schema/career-memory.md).** Not optional — the schema encodes decisions that are invisible from the table names.

## What this owns

The user profile and the activity log: `users`, `skills`, `experience`, `experience_skills`, `projects`, `project_skills`, `education`, `certifications`, `activity_log`, plus intake state (`github_connections`, `linkedin_imports`, `uploaded_files`).

This is the **source of truth for who the user is**. Four other modules read from it. Breaking a shape here breaks them.

## Hard rules

- **`activity_log` is append-only.** Never update, never delete.
- **Dedupe on `eventId`.** `POST /activity` and every webhook receiver require a client-generated UUID and enforce a unique index. Duplicate deliveries are normal.
- **Exports are derived.** Markdown/JSON output is generated on request from the tables. Never treat an export as authoritative, never write back into one.
- **`skills` is `unique(user_id, name)`.** Repeated resume and GitHub parses will collide by design — upsert.
- **JSONB is for opaque data only.** `bullets` and event `payload` are JSONB because nothing queries into them. If another module needs to query it, it gets a column.
- **`github_connections.access_token` is encrypted at rest.** Anything touching it needs sign-off.
- **LinkedIn: manual sync only.** Fetch on explicit user action. Never a timer, never a background scrape. ToS and account-ban risk — this is not a preference.
- **Index `user_id` on every child table.**

## API

`/api/v1/users/:userId/...` — REST/JSON, shared error envelope from `shared/`, UUIDv4 IDs, JWT verified independently on every request.

Other modules consume these endpoints. Additive changes are fine; anything that could break a consumer is a conversation first — see [`/.agents/boundaries.md`](../.agents/boundaries.md).

## Not decided yet

The event bus that feeds `activity_log` is undesigned — see [`open-questions.md`](../docs/open-questions.md) item 4. Phase 1 may use synchronous `POST /activity` and swap a bus in later; confirm before building bus infrastructure.
