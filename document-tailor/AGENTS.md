# document-tailor — agent rules

Read [`/AGENTS.md`](../AGENTS.md) first. This adds rules specific to `document-tailor/`.

## What this owns

Generated documents and their version history. Its own database.

## Hard rules

- **Read source data over REST.** Profile from `career-memory`, analysis from `jd-analysis`. Never query their databases, never keep a local copy of profile data.
- **Never invent profile content.** Everything in a generated document must trace to a real Career Memory entry. Fabricated experience on a real user's resume is the worst failure mode this product has.
- **Version history is append-only.** A regeneration creates a new version; it never overwrites an earlier one.
- **Emit activity events** for generated documents, with a client-generated `eventId`.

## Using the evidence chain

Career Memory's `experience_skills` / `project_skills` join tables link a skill to the *specific role or project* where it was used. Use them — that's how a resume bullet gets picked to back a claimed skill, rather than asserting a skill with nothing behind it. See the [schema](../docs/schema/career-memory.md).

## LLM usage

LLM-heavy module. Provider and integration approach are **not decided** — [`open-questions.md`](../docs/open-questions.md) item 1. Don't pick one by adding a client library.

## Not decided yet

- PDF/DOCX generation approach — open, and it constrains the stack choice for this service
- Where generated files are stored. Blob storage doesn't exist yet — see [`open-questions.md`](../docs/open-questions.md) item 7.
