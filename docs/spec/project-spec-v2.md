# LazyCareer — Project Specification

**Version 0.1 · Draft** · Team size: 4 · Timeline: TBD pending team split

> **⚠️ Status: historical ideation document.**
> This is the original product spec, converted from `LazyCareer_ProjectSpec_v2.docx`
> (kept alongside this file as the archived source).
>
> **Where this document and an ADR disagree, the ADR wins.** The spec left several
> questions deliberately open; [`docs/decisions/`](../decisions/) has since closed
> them. Known conflicts are listed in [`.agents/rules.md`](../../.agents/rules.md).
>
> Read this for **product intent and scope**. Read the ADRs for **how we are actually building it**.

---

## Overview

LazyCareer is a long-term, modular product that solves the fragmented and painful experience of job searching and applying. Each feature is designed to ship as an independent app, with integration happening progressively across defined phases.

The product is built around a central insight: **the job application process has no continuity.** Users switch between spreadsheets, job boards, document editors, and career advice tools with no thread connecting them. LazyCareer replaces that fragmentation with a unified pipeline backed by a living, auto-updating profile of everything the user has done.

## Problems

The following eight problems define the scope of what LazyCareer solves:

1. No single source of truth for experience & activity
2. No understanding of what a role actually requires
3. No structured process for finding roles that match me
4. **4a.** Don't know the steps or procedures for applying to a position
5. **4b.** Can't track which application is at what stage
6. No self-generated feedback loop — no way to reflect on resume quality, role fit, skill gaps, or application patterns
7. Career-related output (profile, resume, cover letter, posts) has to be produced from scratch each time
8. No pipeline for fast, end-to-end application — the process is fragmented across disconnected tools
9. Too lost in the market — no visibility into trends, demand, or a personal strategic game plan

## Problem → Solution Map

| # | Problem | Solution | Phase |
|---|---|---|---|
| 1 | No single source of truth for experience & activity | Automated Career Memory | 1 |
| 2 | No understanding of what a role actually requires | JD Analysis & Consultation | 1 |
| 3 | No structured process for finding roles that match me | Job Search & Filter | 1 |
| 4a | Don't know the steps or procedures for applying | Position Procedure Guide | 2 |
| 4b | Can't track which application is at what stage | Progress Tracker | 2 |
| 5 | No self-generated feedback loop | Application Intelligence & Skill Gap Analyzer | 4 |
| 6 | Career output has to be produced from scratch each time | Career Document Tailor | 2 |
| 7 | No pipeline for fast, end-to-end application | Unified Application Pipeline | 3 |
| 8 | Too lost in the market — no visibility into trends or game plan | Market Intelligence & Career Strategy | 4 |

> **Note:** Problems 4a and 4b ship together as a single module (Application Manager) because they are inseparable from the user's perspective — a procedure guide without a tracker is just a list, and a tracker without guidance is just a spreadsheet.

## Solutions & Features

Each solution is an independently shippable module.

### Module 1 — Automated Career Memory
**✓ Core · Complexity: Medium**

Maintains a living, structured profile of the user — projects, skills, experience, achievements, courses, certifications — that updates as they use the app. This is the data foundation everything else depends on.

- Onboarding intake: import resume, LinkedIn URL, or manual entry
- Structured profile storage: skills, experience, projects, education, certifications
- Auto-update hooks from other modules (e.g. completing a procedure step logs it automatically)
- Manual add / edit entries
- Export profile as Markdown or JSON

### Module 2 — JD Analysis & Consultation
**✓ Core · Complexity: Low–Medium**

Given a job description, breaks it down into what the role actually requires and advises on fit against the user's Career Memory profile.

- Paste or auto-import JD from a search result or URL
- Extract: required skills, nice-to-haves, seniority signals, culture signals, red flags
- Fit score against Career Memory profile
- Gap summary: what you have vs. what they want
- Consultation output: what to emphasize, what to address

### Module 3 — Job Search & Filter
**✓ Core · Complexity: Medium–High**

Lets users find roles matching their criteria without leaving the app. Supports both API-based discovery and manual input.

- Filter by role, location, experience level, salary, company size, remote/hybrid
- API-based results (Adzuna, JSearch, or similar) for discovery
- Manual input: paste URL or fill a job entry form (fallback & power-user path)
- Each result links directly into JD Analysis
- Save and bookmark roles

> **Note:** Scraping job boards is legally grey and technically fragile. The dual approach (API + manual) ensures the module is useful from day one regardless of API coverage.

### Module 4 — Application Manager (4a + 4b)
**✓ Core · Complexity: Low–Medium**

Ships 4a (Position Procedure Guide) and 4b (Progress Tracker) as a single cohesive module. Together they answer: *what do I need to do, and where am I in doing it?*

- Standard procedure templates by role type (SWE, PM, design, etc.)
- AI-generated company-specific notes where data is available
- Checklist of action items per stage (tailor resume, prep for OA, research company…)
- Kanban + table view: Applied → OA → Phone Screen → Interview → Offer → Rejected/Ghosted
- Per-application task list with deadlines
- Status updates trigger Career Memory log entries automatically
- Deadline reminders and notes per application

### Module 5 — Career Document Tailor
**✓ Core · Complexity: Medium**

Generates tailored resumes, cover letters, and LinkedIn posts from the user's Career Memory profile cross-referenced with a specific JD.

- Resume generator: pulls relevant profile entries, formats to match JD requirements
- Cover letter generator: narrative built from profile + JD fit analysis
- LinkedIn post generator: turns logged activities into publishable draft posts
- Export as PDF, DOCX, or plain text
- Version history per application

### Module 6 — Unified Application Pipeline
**✓ Core · Complexity: Medium**

The UX wrapper that connects all modules into one continuous flow. Transforms a collection of tools into a product.

- Single entry point: search or paste a job to begin
- Linear flow: Search → JD Analysis → Procedure Guide → Tracker → Document Tailor
- Passive Career Memory updates at every step
- Dashboard: active applications, upcoming deadlines, recent activity, fit scores at a glance
- State continuity: user context (`userId`, `jobId`, `applicationId`) persists across modules

### Module 7 — Application Intelligence & Skill Gap Analyzer
**V2 Stretch · Complexity: Medium**

Turns accumulated application history into self-generated feedback and a skill development roadmap. Gets smarter over time as more data exists.

- Response rate analytics by role type, source, seniority, company size
- Fit score history across all applications
- Aggregate skill gap: skills most frequently required but missing from profile
- Resume quality score before submission
- Pattern summary: what's not working and why

### Module 8 — Market Intelligence & Career Strategy
**V2 Stretch · Complexity: High**

Gives the user an outside-in view of the job market and a personalized strategic game plan cross-referenced against their Career Memory.

- Trending skills and roles in user's target area
- Demand signals for their target stack and domain
- Cross-reference with Career Memory: where you stand vs. the market
- Skill gap roadmap: prioritized list of what to learn next
- Game plan output: 30/60/90 day strategy based on profile and goals

## Shippable Units

| Ship Order | Module | Complexity | MVP? | Depends On |
|---|---|---|---|---|
| 1 | Career Memory | Medium | ✓ Core | Nothing |
| 2 | JD Analysis & Consultation | Low–Med | ✓ Core | Career Memory |
| 3 | Job Search & Filter | Med–High | ✓ Core | External API |
| 4 | Application Manager (4a + 4b) | Low–Med | ✓ Core | Career Memory |
| 5 | Career Document Tailor | Medium | ✓ Core | Career Memory |
| 6 | Unified Application Pipeline | Medium | ✓ Core | All modules |
| 7 | Application Intelligence | Medium | V2 Stretch | Tracker history |
| 8 | Market Intelligence & Strategy | High | V2 Stretch | External data |

## Architecture Approach

### Microservices

Each module maps naturally to a service boundary. The product architecture is microservices, arrived at organically from the product structure rather than imposed upfront.

- **Independent deployability:** each module ships, updates, and scales on its own
- **Single responsibility:** each service does one thing and owns its data
- **Team autonomy:** team members can own different services without conflicts
- **Phase 3 = API Gateway:** the Unified Pipeline is the orchestration layer, a standard microservices pattern

### Phase 0 — Pre-Phase Contracts

Before Phase 1 begins, the team must agree on three integration contracts. Getting these right upfront determines whether Phase 3 is a 2-week connect job or a 2-month refactor.

1. **Shared Data Schema:** Career Memory data shape agreed on and typed. Every module that reads or writes user experience/activity data uses the same structure.
2. **Shared API Contract:** modules expose clean REST interfaces. The Pipeline calls these in Phase 3 — it doesn't reimplement anything.
3. **Shared Auth & User Identity:** one JWT issued at login, verified by every service independently. Single auth provider (e.g. Supabase Auth or Clerk) used everywhere.

The `shared/` package (types, schema validators, auth helpers, UI kit) is the only place agreed data shapes live. Every service imports from it, never duplicates it.

### Repo Structure

> **⚠️ Superseded.** The spec proposed "single repos per module." We build in a **monorepo** — see [ADR 0007](../decisions/0007-repo-structure.md). The folder list below still stands.

```
shared/               — schema, UI kit, auth helpers (Phase 0 output)
career-memory/
job-search/
jd-analysis/
application-manager/
document-tailor/
pipeline/             — Phase 3 integration layer
app-intelligence/     — Phase 4
market-intelligence/  — Phase 4
```

### Key Architectural Decisions

- **Inter-service communication:** REST for request/response flows; async events (e.g. Redis pub/sub or BullMQ) for Career Memory passive updates. Every service emits activity events; Career Memory consumes them.
- **Database per service:** no shared tables. Services communicate via API, never via direct DB joins.
- **Local development:** Docker Compose so every service runs together in one command.
- **Deployment:** each service deployed independently to Railway or Render (not Kubernetes at this stage).

## Development Phases

Timelines are TBD pending team split decisions. Phases are defined by **what ships**, not by calendar duration.

### Phase 0 — Integration Contracts
- Agree on Career Memory data schema and types
- Define shared API contract format (REST conventions)
- Select and configure auth provider
- Bootstrap `shared/` package and UI kit
- Establish environment and config conventions across repos

*Est. 1–2 weeks. Everyone works from this before splitting. Saves months later.*

### Phase 1 — Foundation & Discovery
- **Career Memory** (data foundation — builds in parallel with others)
- **Job Search & Filter** (leads in priority within Phase 1)
- **JD Analysis & Consultation** (slots in once Search has something to analyze)

*End state: user can find jobs, understand them, and has a growing profile. All three built in parallel.*

### Phase 2 — Application Layer
- **Application Manager** (4a Procedure Guide + 4b Progress Tracker — shipped as one chunk)
- **Career Document Tailor** (draws from mature Career Memory built in Phase 1)

*End state: user has a complete apply → track → document workflow. Both modules built in parallel.*

### Phase 3 — Pipeline Close
- **Unified Application Pipeline** (connects Phase 1 + 2 into one seamless flow)
- Dashboard, entry point, state continuity, and navigation across all modules

*End state: a collection of tools becomes a product. This phase turns the modules into LazyCareer.*

### Phase 4 — Intelligence Layer (Stretch)
- **Application Intelligence & Skill Gap Analyzer** (parallel with Market Intel)
- **Market Intelligence & Career Strategy Consultant** (parallel)

*End state: the product moves from useful to genuinely smart. Requires accumulated user data from Phases 1–3 to be meaningful.*

## The Unified Application Pipeline Flow

When all modules are integrated, the user experience follows a single continuous flow:

1. Job Search & Filter
2. JD Analysis & Consultation
3. Position Procedure Guide + Progress Tracker
4. Career Document Tailor
5. Application Intelligence & Skill Gap Analyzer

**Running passively in the background across every step:** Automated Career Memory — every action the user takes updates their profile, which feeds back into document generation, fit scoring, post creation, and strategy on demand.

## Next Steps

1. Decide team split (4 people) — this will define timelines for each phase
2. Decide tech stack per module and shared layer
3. Execute Phase 0: agree on shared schema, API contracts, and auth before anyone writes feature code
4. Assign module ownership per team member
5. Begin Phase 1 in parallel across Career Memory, Job Search, and JD Analysis

## Pending Discussions

> Live status of these is tracked in [`docs/open-questions.md`](../open-questions.md).

### Team Split (TBD)

With a team of 4, how ownership is divided across modules directly determines phase timelines and parallelism. Key questions:

- Who owns which module(s)? Does ownership follow phases or skill sets?
- Is there a dedicated `shared/` and Phase 0 owner, or is that a collective responsibility?
- Who owns the Pipeline (Phase 3)? It touches every module and needs someone with full system context.
- Are roles full-stack per module, or split by frontend/backend across modules?

### API Contract, Architecture & Module Implementation (TBD)

Each module's implementation form, the contracts between them, and the overall architecture needs a dedicated design session. This is Phase 0 work.

**Module implementation form** — each module does not have to be a web app. It could be a web app, a REST API / microservice, an MCP server, a skill / prompt layer, or a CLI tool. The right form depends on who consumes it, whether it needs a UI, and how it fits into the Pipeline.

**API contract design** — questions to resolve:
- REST vs. tRPC vs. GraphQL — which communication style across services?
- Shared context shape — what does `{ userId, jobId, applicationId }` look like and who owns it?
- Event bus design — which events does Career Memory listen for, and what triggers them?
- Error handling and versioning conventions — how do modules handle breaking changes?
- Auth propagation — how is the JWT passed and verified across service boundaries?

### Tech Stack (TBD)

Tech stack per module and the shared layer needs to be decided in the architecture session. Considerations include team familiarity, module implementation form, deployment target, and long-term maintainability. This decision feeds directly into team split and Phase 0 setup.
