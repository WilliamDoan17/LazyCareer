# jd-analysis

**Module 2 — JD Analysis & Consultation.** Breaks a job description down into what the role actually requires, and advises on fit against the user's profile.

| | |
|---|---|
| **Kind** | REST API / microservice |
| **Phase** | 1 — slots in once Job Search has something to analyze |
| **Complexity** | Low–Medium |
| **Status** | Empty — not started |
| **Owner** | TBD |
| **Depends on** | career-memory (read) |
| **Depended on by** | document-tailor, pipeline, app-intelligence |

## Responsibility

Solves problem 2: *no understanding of what a role actually requires*.

- Paste or auto-import a JD from a search result or URL
- Extract: required skills, nice-to-haves, seniority signals, culture signals, red flags
- **Fit score** against the Career Memory profile
- **Gap summary**: what you have vs. what they want
- Consultation output: what to emphasize, what to address

## Owns

Analyses and fit scores. Not the job itself (that's `job-search/`) and not the profile (that's `career-memory/`).

## Note on fit scoring

Fit should be **evidence-based**, not a flat skill-name match. Career Memory's `experience_skills` and `project_skills` join tables exist precisely so a claimed skill can be backed by the specific role or project where it was used — see the [schema](../docs/schema/career-memory.md).

## Status

Blocked on the [tech stack decision](../docs/open-questions.md), including the LLM provider choice.

See [`AGENTS.md`](AGENTS.md) · [architecture](../docs/architecture.md)
