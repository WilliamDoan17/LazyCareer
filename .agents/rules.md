# Rules

Binding constraints. Follow them without needing the rationale — the rationale is linked if you want it.

## Precedence

**ADRs > spec > everything else.**

[`docs/decisions/`](../docs/decisions/) is authoritative. [`docs/spec/project-spec-v2.md`](../docs/spec/project-spec-v2.md) is historical ideation and left several questions open that the ADRs have since closed. Where they disagree, the ADR wins — no exceptions, no judgement call.

### Known conflicts — already resolved

| Topic | Spec says | **Binding** |
|---|---|---|
| Repo layout | Single repo per module | **Monorepo with workspaces** ([0007](../docs/decisions/0007-repo-structure.md)) |
| Communication | REST vs tRPC vs GraphQL open | **REST/JSON** ([0005](../docs/decisions/0005-api-conventions.md)) |
| Module form | Open per module | **Career Memory = REST service + thin UI** ([0001](../docs/decisions/0001-career-memory-product-form.md)) |
| UI | Per-module UI open | **One unified frontend, single JWT** ([0006](../docs/decisions/0006-auth-and-ui-architecture.md)) |

If you find a fifth conflict, do not resolve it yourself. Flag it and ask.

## Identifiers

- **UUIDv4** for `userId`, `jobId`, `applicationId`, and every ID that crosses a service boundary.
- **Never** auto-increment integers for anything shared across services. Service-internal-only IDs are the sole exception, and they must never appear in an API response.

[ADR 0004](../docs/decisions/0004-id-format.md)

## APIs

- **REST/JSON only.** No GraphQL, no tRPC, no RPC-over-POST.
- **URI path versioning:** `/api/v1/users/:userId/skills`. Bump to `/v2/` only on breaking changes; additive fields do not require a bump.
- **One error envelope**, imported from `shared/`, never redefined locally:
  ```json
  { "error": { "code": "RESOURCE_NOT_FOUND", "message": "...", "details": {} } }
  ```
- **Status codes semantically:** `400` validation · `401`/`403` auth · `404` not found · `409` conflict · `422` unprocessable · `429` rate limit · `500` internal.

[ADR 0005](../docs/decisions/0005-api-conventions.md)

## Events

- Every event-driven write carries a **client-generated `eventId` (UUID)**. This includes `POST /activity` and every webhook receiver.
- **Dedupe server-side** on a unique index over `eventId`. Duplicate deliveries are normal, not exceptional — never assume exactly-once delivery.
- `activity_log` is **append-only**. Never update or delete a row in it.

[ADR 0005](../docs/decisions/0005-api-conventions.md)

## Data

- **Database per service.** No shared tables. No cross-service database joins. No reading another service's database — call its REST API.
- **Postgres relational tables are the source of truth.** Markdown/JSON profile exports are derived output generated on request; never treat an export as authoritative and never write back into one.
- **JSONB only for opaque data** — raw parsed dumps, event payloads. If another module needs to query or join on it, it gets a column or a table.
- **Index `user_id` on every child table.** It's the near-universal filter.

[ADR 0002](../docs/decisions/0002-storage-model.md) · [Career Memory schema](../docs/schema/career-memory.md)

## Auth

- One auth provider, one JWT, issued at login.
- The frontend attaches it to every request to every service.
- **Every service verifies the JWT independently.** No shared session store, no trusting an upstream service to have checked.

[ADR 0006](../docs/decisions/0006-auth-and-ui-architecture.md)

## Intake

- **GitHub may be polled and may use webhooks.** Official API, safe to keep live.
- **LinkedIn is manual-sync only.** Fetch on explicit user action — initial import or an "Update" button click. **Never** on a timer, never continuously, never scraped in the background. This is a ToS and account-ban risk, not a preference.
- Chat intake is conversational, not a static form: parse freeform input, pre-fill what you can, ask targeted follow-ups only for genuine gaps.

[ADR 0003](../docs/decisions/0003-input-sources.md)

## Shared code

- Types, schema validators, auth helpers, and the UI kit live in **`shared/` only**.
- **Never duplicate a shape across services.** If two services need it, it belongs in `shared/` and both import it.
- `shared/` depends on nothing else in this repo.

## Documentation

- **A merged ADR is immutable.** Superseding one means writing a new ADR that says so — never edit the old one.
- New decision → new ADR + an entry in the [docs index](../docs/README.md).
- New domain term → [`glossary.md`](../docs/glossary.md).
- **One fact, one home.** If it's documented elsewhere, link to it. Never restate it.

## Current state

**Phase 0. No code exists yet.** The tech stack is undecided — do not introduce a language, framework, package manager, ORM, or build tool without an explicit decision. See [`open-questions.md`](../docs/open-questions.md).

---

Next: [`boundaries.md`](boundaries.md) — what to ask about before doing.
