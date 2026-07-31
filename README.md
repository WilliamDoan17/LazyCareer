# LazyCareer

**The job application process has no continuity.**

You track applications in a spreadsheet, find roles on three different boards, rewrite your resume from scratch each time, and forget what you did last month. Nothing connects. Every application starts over.

LazyCareer replaces that with a single pipeline, backed by a living profile of everything you've done — one that updates itself as you use the app.

> **Status: Phase 0.** Contracts are being agreed. No code yet.

---

## What it does

Eight problems, eight modules, one flow.

| # | The problem | What solves it | Phase |
|---|---|---|---|
| 1 | No single source of truth for your experience & activity | **Automated Career Memory** | 1 |
| 2 | No understanding of what a role actually requires | **JD Analysis & Consultation** | 1 |
| 3 | No structured process for finding roles that match you | **Job Search & Filter** | 1 |
| 4a | You don't know the steps for applying to a position | **Position Procedure Guide** | 2 |
| 4b | You can't track which application is at what stage | **Progress Tracker** | 2 |
| 5 | No feedback loop — no read on fit, gaps, or what's not working | **Application Intelligence** | 4 |
| 6 | Every resume, cover letter, and post starts from scratch | **Career Document Tailor** | 2 |
| 7 | The whole process is fragmented across disconnected tools | **Unified Application Pipeline** | 3 |
| 8 | No visibility into market trends or a personal game plan | **Market Intelligence & Strategy** | 4 |

## How it feels to use

```
Find a job  →  Understand what it wants  →  Know the steps  →  Track where you are  →  Send tailored documents
```

And underneath every step, running quietly: **Career Memory**. Every action you take updates your profile — which then feeds back into better documents, sharper fit scores, and a real picture of where you stand.

That's the idea. You do the work once; the app remembers it forever.

## Where it's at

Phase 0 of five. The team is agreeing the shared data schema, API contracts, and auth before anyone writes feature code — the boring part that decides whether the last phase takes two weeks or two months.

| Phase | Ships | Status |
|---|---|---|
| **0** | Shared schema, API contracts, auth, `shared/` package | 🟡 In progress |
| **1** | Career Memory · Job Search · JD Analysis | ⚪ Not started |
| **2** | Application Manager · Document Tailor | ⚪ Not started |
| **3** | Unified Pipeline — the modules become a product | ⚪ Not started |
| **4** | Application Intelligence · Market Intelligence *(stretch)* | ⚪ Not started |

---

## Three doors

**👤 Curious what this is?** You just read it. There's nothing to install yet.

**🛠️ Contributing?** → **[`docs/START-HERE.md`](docs/START-HERE.md)** — 5 minutes, cold start to oriented.

**🤖 An AI agent?** → **[`AGENTS.md`](AGENTS.md)** — rules, constraints, and boundaries.

## Repo

Monorepo. Ten workspaces, each independently deployable.

```
shared/               types, validators, auth helpers, UI kit
frontend/             the unified web app

career-memory/        Module 1 — the profile everything depends on
jd-analysis/          Module 2 — what a role really wants
job-search/           Module 3 — finding roles
application-manager/  Module 4 — procedure guide + tracker
document-tailor/      Module 5 — resumes, cover letters, posts
pipeline/             Module 6 — the flow that connects them
app-intelligence/     Module 7 — feedback loop (stretch)
market-intelligence/  Module 8 — market view (stretch)

docs/                 architecture, decisions, schema, conventions
.agents/              rules for AI agents working in this repo
```

Every module folder has its own `README.md`.

## Docs

- [Architecture](docs/architecture.md) — how the pieces fit
- [Decisions](docs/decisions/) — every architectural choice and why
- [Glossary](docs/glossary.md) — what our terms mean
- [Open questions](docs/open-questions.md) — what's still undecided
- [Project spec](docs/spec/project-spec-v2.md) — original product ideation

## License

See [LICENSE](LICENSE).
