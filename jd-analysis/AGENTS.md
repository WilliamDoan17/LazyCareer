# jd-analysis — agent rules

Read [`/AGENTS.md`](../AGENTS.md) first. This adds rules specific to `jd-analysis/`.

## What this owns

JD analyses, fit scores, gap summaries. Its own database.

## Hard rules

- **Read the profile over REST.** Call `career-memory`'s `/api/v1/...` endpoints. Never query its database, never keep a local copy of profile data.
- **Fit scores must be evidence-based.** Back a matched skill with the specific experience or project where it was used, via Career Memory's `experience_skills` / `project_skills`. A flat skill-name match is not a fit score.
- **Emit activity events** for analyses the user runs, so Career Memory logs them. Include a client-generated `eventId`.
- **Never write to Career Memory directly** beyond the activity event path.

## LLM usage

This module is LLM-heavy. The provider and integration approach are **not decided** — see [`open-questions.md`](../docs/open-questions.md) item 1. Don't pick one by adding a client library.

When it is decided: extraction output must be structured and validated against a shape in `shared/`. Freeform text that another module has to parse is a contract violation.

## Not decided yet

Whether JDs are persisted here or in `job-search/` needs settling with whoever owns that module — a JD can arrive from a search result or a direct paste, and the two paths shouldn't produce two sources of truth. Ask before assuming.
