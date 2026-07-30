# Career Memory (Module 1)

Maintains a living, structured profile of the user — projects, skills, experience, achievements, courses, certifications — that updates as they use the app. This is the data foundation everything else depends on.

## Product Form
REST API / microservice, with a thin web UI for onboarding, manual edit, and export. Not a standalone web app, not an MCP server, not a pure skill/prompt layer.

Other modules — JD Analysis, Application Manager, Career Document Tailor — read and write Career Memory data programmatically, so it needs a stable API contract other services call. The UI is a secondary consumer of that same API, not a separate product. See [API_Contracts.md](API_Contracts.md) and [Auth_And_UI.md](Auth_And_UI.md) for the conventions and auth model it follows.

## Storage Model
Relational DB schema per user (Postgres), not a per-user file. Other modules need atomic partial updates — add one skill, log one activity event — without a read-modify-write race on a single blob. A relational schema also gives queryability (e.g. join skills to the experience that used them) a flat file can't.

Markdown/JSON export is a **generated output** (a feature), not the source of truth. The DB is authoritative; exports are derived on request.

## Input Sources
1. **User input via chat** — conversational onboarding/update interface, not a static form. Parses freeform input, pre-fills structured entries, asks targeted follow-up questions only for gaps.
2. **Resume files** — PDF, DOCX, TeX, etc. Parsed into structured profile entries.
3. **Logging files** — PDF/DOCX/TeX documents describing a project or career progress. Parsed and appended to the profile/activity log.
4. **GitHub (live)** — OAuth connect once; fetch full history on first import, then live updates via webhooks/polling (projects, languages/skills, activity).
5. **LinkedIn (manual sync)** — fetch profile once via URL paste at import; re-sync only when the user clicks a manual "Update" button. **Not** continuously scraped — LinkedIn has no public API for polling activity/profile changes, and continuous scraping risks ToS violations and account bans (same risk class the project spec flags for job-board scraping).

Other LazyCareer modules also push passive events (e.g. application status change, procedure step completed) into `activity_log` automatically.

## Schema (Postgres)

Pattern: normalized tables for anything other modules need to query/join on, JSONB only for opaque/raw data (parsed source dumps, event payloads).

### `users`
| column | type | notes |
|---|---|---|
| id | uuid pk | |
| email | text | |
| name | text | |
| created_at | timestamptz | |
| updated_at | timestamptz | |

Auth (password, sessions) lives in the shared auth provider (see [Auth_And_UI.md](Auth_And_UI.md)), not here — this table just anchors the `userId` every other table references.

### `skills`
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

### `experience`
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

### `experience_skills` (join table)
| column | type |
|---|---|
| experience_id | uuid fk -> experience.id |
| skill_id | uuid fk -> skills.id |

Links a skill to the *specific role* where it was used — not just "I know React" globally, but "React was used at this role." Lets Career Document Tailor pick the right experience bullet to back up a claimed skill when tailoring to a JD, and lets fit-scoring be evidence-based rather than a flat skill-name match.

### `projects`
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

### `project_skills` (join table)
Same shape as `experience_skills`, joining `project_id` to `skill_id`.

### `education`
| column | type |
|---|---|
| id | uuid pk |
| user_id | uuid fk -> users.id |
| institution | text |
| degree | text |
| field | text |
| start_date | date |
| end_date | date null |

### `certifications`
| column | type |
|---|---|
| id | uuid pk |
| user_id | uuid fk -> users.id |
| name | text |
| issuer | text |
| issue_date | date |
| expiry_date | date null |

### `activity_log` (event-sourced, append-only)
| column | type | notes |
|---|---|---|
| id | uuid pk | |
| user_id | uuid fk -> users.id | |
| type | text | e.g. "status_change", "procedure_step_completed", "github_commit" |
| source | text | which module/service emitted it |
| payload | jsonb | |
| event_id | uuid unique | idempotency key — see [API_Contracts.md](API_Contracts.md) |
| created_at | timestamptz | |

### Intake / connection state
Separate from profile data — tracks the state of each input source.

**`github_connections`**
| column | type |
|---|---|
| user_id | uuid pk/fk -> users.id |
| access_token | text (encrypted at rest) |
| github_username | text |
| connected_at | timestamptz |
| last_synced_at | timestamptz |

**`linkedin_imports`**
| column | type |
|---|---|
| user_id | uuid pk/fk -> users.id |
| profile_url | text |
| last_synced_at | timestamptz |
| raw_data | jsonb — last-fetched raw payload, for debugging/re-parsing |

**`uploaded_files`**
| column | type | notes |
|---|---|---|
| id | uuid pk | |
| user_id | uuid fk -> users.id | |
| file_type | enum(resume, log) | |
| file_url | text | pointer to blob storage, not the file itself |
| parsed_at | timestamptz null | |
| raw_text | text null | extracted text, kept for re-parsing if extraction logic improves |

### Indexing
- `user_id` indexed on every child table (the near-universal filter).
- `activity_log.event_id` unique index for idempotency.
- `skills(user_id, name)` unique to prevent duplicates from repeated resume/GitHub parses.
