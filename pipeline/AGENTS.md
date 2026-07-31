# pipeline — agent rules

Read [`/AGENTS.md`](../AGENTS.md) first. This adds rules specific to `pipeline/`.

## What this owns

Cross-module session context — the `{ userId, jobId, applicationId }` that follows a user through the flow. **No domain data.**

## The one hard rule

**Reimplement nothing.** Every module exposes a REST API; the pipeline calls it. If you find yourself computing a fit score, formatting a document, or deciding an application's next stage here, that logic belongs in the module that owns it — stop and put it there.

A shortcut taken here is the failure this whole architecture was designed to avoid.

## Other rules

- **No module's data lives here.** Read it from the owning service on demand.
- **Never query another service's database.** REST only, always.
- **A missing module must degrade, not crash.** The pipeline touches everything, so it fails the most ways. One module being down should break one step, not the flow.

## Not decided yet

Most of this module's foundation is open. Confirm before building:

- **Shared context shape and ownership** — [`open-questions.md`](../docs/open-questions.md) item 5. Exact fields, who mints it, how it's transported, and what happens when a module is entered without full context.
- **Service-to-service auth** — item 6. The pipeline is the heaviest cross-service caller; it needs this settled first.

## Note on ownership

This module touches every other one and needs someone holding full system context — see the team split question in [`open-questions.md`](../docs/open-questions.md).
