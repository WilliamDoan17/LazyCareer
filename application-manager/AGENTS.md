# application-manager — agent rules

Read [`/AGENTS.md`](../AGENTS.md) first. This adds rules specific to `application-manager/`.

## What this owns

Applications, their stage, tasks, deadlines, and notes. Its own database. `applicationId` is UUIDv4, minted here.

## Hard rules

- **Use the canonical stage sequence** — `Applied → OA → Phone Screen → Interview → Offer → Rejected/Ghosted`. Don't invent stages or rename them; other modules and analytics depend on these values. See [`glossary.md`](../docs/glossary.md).
- **Every status change emits an activity event** to Career Memory with a client-generated `eventId`. This is the module's main contribution to the passive-update mechanic — it is not optional polish.
- **Never write to Career Memory's database.** Events only, or its REST API.
- **4a and 4b are one module.** Don't split them into separate services; they were deliberately merged.

## Events

This module is the heaviest event emitter in the product. Fire-and-forget: a Career Memory outage must not block a status update. Career Memory dedupes on `eventId`, so retrying is safe and expected.

## Not decided yet

- The event bus itself is undesigned — [`open-questions.md`](../docs/open-questions.md) item 4. Phase 1 may use synchronous `POST /activity`. Confirm before building bus infrastructure.
- The full event catalogue (which events exist, what payload each carries) needs agreeing with the Career Memory owner before this module emits anything.
