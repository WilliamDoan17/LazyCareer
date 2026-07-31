# document-tailor

**Module 5 — Career Document Tailor.** Generates tailored resumes, cover letters, and LinkedIn posts from the user's profile cross-referenced with a specific JD.

| | |
|---|---|
| **Kind** | REST API / microservice |
| **Phase** | 2 |
| **Complexity** | Medium |
| **Status** | Empty — not started |
| **Owner** | TBD |
| **Depends on** | career-memory (read), jd-analysis (read) |
| **Depended on by** | pipeline |

## Responsibility

Solves problem 6: *career output has to be produced from scratch each time*.

- **Resume generator** — pulls relevant profile entries, formats to match JD requirements
- **Cover letter generator** — narrative built from profile + JD fit analysis
- **LinkedIn post generator** — turns logged activities into publishable draft posts
- Export as PDF, DOCX, or plain text
- Version history per application

## Owns

Generated documents and their version history. Its own database. Not the profile, not the analysis.

## Why Phase 2

Draws from a *mature* Career Memory built during Phase 1. Generating documents from a thin profile produces thin documents.

## Status

Blocked on the [tech stack decision](../docs/open-questions.md), including the LLM provider and a PDF/DOCX generation approach.

See [`AGENTS.md`](AGENTS.md) · [architecture](../docs/architecture.md)
