# Event Schema

Minimal, primitive-level contract for events written to Career Memory's `activity_log` (see [Career_Memory.md](Career_Memory.md)). Kept intentionally loose — extend as modules ship, don't pre-design event types for modules that don't exist yet.

## Envelope
```json
{
  "event_id": "uuid",      // client-generated, for idempotency
  "type": "string",        // e.g. "status_change"
  "source": "string",      // emitting module, e.g. "application-manager"
  "payload": {},           // free-form, shape depends on type
  "timestamp": "ISO8601"
}
```

## Known types (starter set — add more as modules ship)
| type | emitted by | payload (rough) |
|---|---|---|
| `status_change` | Application Manager | `{ applicationId, fromStage, toStage }` |
| `procedure_step_completed` | Application Manager | `{ applicationId, step }` |
| `document_generated` | Career Document Tailor | `{ applicationId, documentType }` |
| `github_commit` | Career Memory (internal, from GitHub sync) | `{ repo, commitSha, languages }` |

## Rule
Any new module that wants to log an activity picks a `type` string and documents it here — one line added to the table above, not a new schema file per event.
