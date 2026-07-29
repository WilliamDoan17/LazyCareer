# 0005 — API Conventions: REST, Versioning, Error Handling

## Decision: REST over GraphQL/tRPC
All inter-service and frontend-to-service communication uses REST/JSON.

**Why**: Team members may use different tech stacks per module (spec leaves this open). REST/JSON requires zero shared tooling to consume. tRPC would lock every consumer into TypeScript end-to-end. GraphQL needs a schema/resolver layer and typically a federation gateway to do well across services — too much infra overhead for a 4-person team's first pass. Career Memory's data is also fundamentally CRUD over bounded resources (skills, experience, projects...), which REST models cleanly without needing GraphQL's flexible graph querying.

## Decision: URI path versioning
`/api/v1/users/:userId/skills`, etc. Bump to `/v2/` only on breaking changes; additive fields don't require a bump.

**Why**: Visible in logs, curl-able, no shared parsing logic needed across polyglot services (vs. header-based versioning).

## Decision: Shared error envelope
Defined once in `shared/`, reused by every service:
```json
{ "error": { "code": "RESOURCE_NOT_FOUND", "message": "...", "details": {} } }
```
Status codes used semantically: `400` validation, `401`/`403` auth, `404` not found, `409` conflict, `422` unprocessable, `429` rate limit, `500` internal.

## Decision: Idempotency for event-driven writes
`POST /activity` and the GitHub webhook receiver require a client-generated `eventId` (UUID). Retries/duplicate deliveries are normal for event-driven writes — dedupe on `eventId` server-side (unique index in `activity_log`).
