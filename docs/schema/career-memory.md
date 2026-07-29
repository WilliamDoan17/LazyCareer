# Career Memory — DB Schema (Postgres)

Pattern: normalized tables for anything other modules need to query/join on, JSONB only for opaque/raw data (parsed source dumps, event payloads). See [0002-storage-model](../decisions/0002-storage-model.md) and [0004-id-format](../decisions/0004-id-format.md).

## `users`
| column | type | notes |
|---|---|---|
| id | uuid pk | |
| email | text | |
| name | text | |
| created_at | timestamptz | |
| updated_at | timestamptz | |

Auth (password, sessions) lives in the shared auth provider (Supabase/Clerk), not here — see [0006-auth-and-ui-architecture](../decisions/0006-auth-and-ui-architecture.md). This table just anchors the `userId` every other table references.

## `skills`
| column | type | notes |
|---|---|---|
| id | uuid pk | |
| user_id | uuid fk -> users.id | |
| name | text | |
| category | text | e.g. "language", "framework", "soft skill" |
| source | enum(manual, resume, inferred, github) | |
| last_used | date null | |
| created_at | timestamptz | |

`unique(user_id, name)`

## `experience`
| column | type | notes |
|---|---|---|
| id | uuid pk | |
| user_id | uuid fk -> users.id | |
| role | text | |
| company | text | |
| start_date | date | |
| end_date | date null | |
| bullets | jsonb | array of strings — no other module queries into individual bullets |
| created_at, updated_at | timestamptz | |

## `experience_skills` (join table)
| column | type |
|---|---|
| experience_id | uuid fk -> experience.id |
| skill_id | uuid fk -> skills.id |

Links a skill to the *specific role* where it was used — not just "I know React" globally, but "React was used at this role." Lets Career Document Tailor pick the right experience bullet to back up a claimed skill when tailoring to a JD, and lets fit-scoring be evidence-based rather than a flat skill-name match.

## `projects`
| column | type | notes |
|---|---|---|
| id | uuid pk | |
| user_id | uuid fk -> users.id | |
| name | text | |
| description | text | |
| link | text null | |
| repo_url | text null | populated when source = github |
| source | enum(manual, github) | |
| created_at, updated_at | timestamptz | |

## `project_skills` (join table)
Same shape as `experience_skills`, joining `project_id` to `skill_id`.

## `education`
| column | type |
|---|---|
| id | uuid pk |
| user_id | uuid fk -> users.id |
| institution | text |
| degree | text |
| field | text |
| start_date | date |
| end_date | date null |

## `certifications`
| column | type |
|---|---|
| id | uuid pk |
| user_id | uuid fk -> users.id |
| name | text |
| issuer | text |
| issue_date | date |
| expiry_date | date null |

## `activity_log` (event-sourced, append-only)
| column | type | notes |
|---|---|---|
| id | uuid pk | |
| user_id | uuid fk -> users.id | |
| type | text | e.g. "status_change", "procedure_step_completed", "github_commit" |
| source | text | which module/service emitted it |
| payload | jsonb | |
| event_id | uuid unique | idempotency key — see [0005-api-conventions](../decisions/0005-api-conventions.md) |
| created_at | timestamptz | |

## Intake / connection state
Separate from profile data — tracks the state of each input source (see [0003-input-sources](../decisions/0003-input-sources.md)).

### `github_connections`
| column | type |
|---|---|
| user_id | uuid pk/fk -> users.id |
| access_token | text (encrypted at rest) |
| github_username | text |
| connected_at | timestamptz |
| last_synced_at | timestamptz |

### `linkedin_imports`
| column | type |
|---|---|
| user_id | uuid pk/fk -> users.id |
| profile_url | text |
| last_synced_at | timestamptz |
| raw_data | jsonb — last-fetched raw payload, for debugging/re-parsing |

### `uploaded_files`
| column | type | notes |
|---|---|---|
| id | uuid pk | |
| user_id | uuid fk -> users.id | |
| file_type | enum(resume, log) | |
| file_url | text | pointer to blob storage, not the file itself |
| parsed_at | timestamptz null | |
| raw_text | text null | extracted text, kept for re-parsing if extraction logic improves |

## Indexing
- `user_id` indexed on every child table (the near-universal filter).
- `activity_log.event_id` unique index for idempotency.
- `skills(user_id, name)` unique to prevent duplicates from repeated resume/GitHub parses.
