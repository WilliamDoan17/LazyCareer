# API Contracts

Conventions every module's API follows, so the Phase 3 Pipeline can integrate them without reimplementing anything.

## Style: REST
All inter-service and frontend-to-service communication uses REST/JSON, not GraphQL or tRPC.

Team members may use different tech stacks per module. REST/JSON requires zero shared tooling to consume — tRPC would lock every consumer into TypeScript end-to-end, and GraphQL needs a schema/resolver layer (typically a federation gateway) that's too much infra for a 4-person team's first pass. Module data is also fundamentally CRUD over bounded resources (skills, experience, applications...), which REST models cleanly.

## Versioning
URI path versioning: `/api/v1/users/:userId/skills`, etc. Bump to `/v2/` only on breaking changes; additive fields don't require a bump.

Visible in logs, curl-able, no shared parsing logic needed across polyglot services (vs. header-based versioning).

## Error envelope
Defined once in `shared/`, reused by every service:
```json
{ "error": { "code": "RESOURCE_NOT_FOUND", "message": "...", "details": {} } }
```
Status codes used semantically: `400` validation, `401`/`403` auth, `404` not found, `409` conflict, `422` unprocessable, `429` rate limit, `500` internal.

## Idempotency
Event-driven writes (e.g. Career Memory's `POST /activity`, GitHub webhook receivers) require a client-generated `eventId` (UUID). Retries and duplicate deliveries are normal for event-driven writes — dedupe on `eventId` server-side via a unique index.

## Shared types
There's no separate shared types package to design — the "shared type" for user profile data *is* [Career_Memory.md](Career_Memory.md)'s API/schema shape (Skill, Experience, Project, etc.). Any module reading/writing profile data uses those types directly; they aren't redefined elsewhere.

## Shared context / ID format
`userId`, `jobId`, `applicationId` are all standard **UUIDv4** (36-char). Each service owns its own database and generates IDs independently — there's no central ID authority across microservices, so UUIDv4 gives effectively zero collision risk with no coordination required.

## Event bus
Async events (e.g. Redis pub/sub or BullMQ) carry passive updates — every service emits activity events; Career Memory consumes them. REST stays reserved for request/response flows.
