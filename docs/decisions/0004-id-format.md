# 0004 — ID Format

## Decision
`userId`, `jobId`, and `applicationId` are all standard **UUIDv4** (36-char, e.g. `018f4d2e-...`).

## Why
Each service owns its own database and generates IDs independently — there's no central ID authority across microservices. UUIDv4 gives effectively zero collision risk with no coordination required between services.

## Consequence
All schemas use `uuid` as the primary key type for these fields. No auto-increment integer IDs for anything shared across service boundaries.
