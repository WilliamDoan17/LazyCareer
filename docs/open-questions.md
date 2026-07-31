# Open Questions

Decisions that are **not yet made**. Everything here blocks something downstream.

When one is resolved: write an [ADR](decisions/), then strike the entry here with a link to it. This file only ever holds live questions.

> Anything not listed here that you think is undecided is probably already
> decided — check [`decisions/`](decisions/) first.

---

## 🔴 Blocking Phase 1

### 1. Tech stack per module and for `shared/`
**Blocks:** everything — build tooling, CI, `shared/` package format, all "how do I run this" documentation, and half of team split.

The spec leaves this fully open. Considerations named there: team familiarity, module implementation form, deployment target, long-term maintainability.

Open sub-questions:
- One language across all services, or polyglot per module? [ADR 0005](decisions/0005-api-conventions.md) chose REST partly *to keep polyglot possible* — do we actually want to use that freedom?
- If TypeScript: which workspace tool (npm / pnpm / yarn), and Turborepo or Nx or neither? [ADR 0007](decisions/0007-repo-structure.md) leaves this open deliberately.
- Postgres access layer per service — ORM, query builder, or raw?
- LLM provider and integration approach for the AI-heavy modules (JD Analysis, Document Tailor, Market Intelligence).

Until this lands, every command section in the docs is a TODO stub.

---

### 2. Team split — who owns what
**Blocks:** all phase timelines, module assignment, and PR review routing.

Four people, eight modules. The spec flags this as needing a dedicated session. Questions to answer:
- Does ownership follow phases or skill sets?
- Is there a dedicated `shared/` + Phase 0 owner, or is it collective?
- Who owns `pipeline/` (Phase 3)? It touches every module and needs someone holding full system context.
- Full-stack per module, or split frontend/backend across modules?

Every module `README.md` currently says `Owner: TBD`. Those fill in from this.

---

### 3. Auth provider selection
**Blocks:** `shared/` auth helpers, every service's JWT verification, frontend login.

[ADR 0006](decisions/0006-auth-and-ui-architecture.md) decided the *architecture* — one provider, one JWT, verified independently by every service — but named Supabase Auth and Clerk only as examples. The actual pick is open.

Needs: cost at expected scale, student-friendly onboarding, social login support (Google especially), and how cleanly it verifies tokens in whichever runtimes the stack decision produces.

---

## 🟡 Blocking Phase 2+

### 4. Event bus design
**Blocks:** every passive Career Memory update — the product's central mechanism.

The spec suggests Redis pub/sub or BullMQ. [ADR 0005](decisions/0005-api-conventions.md) settled idempotency (`eventId` + unique index), but the bus itself is undesigned:

- Which implementation? (Redis pub/sub, BullMQ, Postgres LISTEN/NOTIFY, or plain HTTP callbacks to start?)
- The full event catalogue: which events exist, what payload shape each carries, which module emits it.
- Delivery guarantees — at-least-once is assumed by the idempotency design, but is it enforced or hoped for?
- What happens to events emitted while Career Memory is down? Retry policy, dead-letter handling.
- Do we need the bus in Phase 1 at all, or can Phase 1 modules call `POST /activity` synchronously and swap in a bus later?

That last question is worth answering early — it may let Phase 1 ship without any bus infrastructure.

---

### 5. Shared context shape and ownership
**Blocks:** Phase 3 pipeline, and any cross-module navigation before it.

`{ userId, jobId, applicationId }` follows the user across modules. Undecided:
- Exact shape — is it exactly those three fields, or does it carry more?
- Who owns and issues it — `pipeline/`, or does it exist before Phase 3?
- How is it transported — URL params, headers, client state?
- What happens when a module is entered directly without full context (e.g. JD Analysis with no `applicationId` yet)?

`jobId` is especially unclear: jobs come from external APIs *and* manual entry, so who mints the ID and when a job becomes persistent both need answering.

---

### 6. JWT propagation mechanics
**Blocks:** service-to-service calls, which start in Phase 1 (JD Analysis reads Career Memory).

[ADR 0006](decisions/0006-auth-and-ui-architecture.md) covers frontend → service. Service → service is not covered:
- Does a service forward the user's JWT when calling another service, or use its own service credential?
- If forwarded: how is expiry handled mid-request-chain?
- Are there endpoints only callable service-to-service, never from the frontend? How are they protected?

---

## 🟢 Needed before first deploy

### 7. Deploy target confirmation
**Blocks:** CI configuration, per-service Dockerfiles.

The spec names Railway or Render, explicitly not Kubernetes at this stage. Not yet confirmed as a decision, and no ADR records it. Also open: where Postgres is hosted per service, and where uploaded files (resumes, logging files) go — `uploaded_files.file_url` in the [Career Memory schema](schema/career-memory.md) points at blob storage that does not exist yet.

### 8. CI setup
**Blocks:** nothing yet; blocks merge confidence as soon as code exists.

[ADR 0007](decisions/0007-repo-structure.md) specifies one pipeline definition with per-folder jobs. Which CI provider, and what gates run on a PR, are open — and partly downstream of the tech stack decision.

---

## Resolved

*(Nothing yet. Resolved entries move here with a link to the ADR that closed them.)*
