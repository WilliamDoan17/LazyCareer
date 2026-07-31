# Start Here

You're new to LazyCareer and you want to be useful today. This page is the route.

**Total: 5 minutes.** Follow it in order — each step assumes the one before it.

---

## The 5-minute path

### 1 · What are we building? — *1 min*
→ [`../README.md`](../README.md)

Eight problems, eight modules, four phases. Skim the problem → module table. Don't memorise it; you'll absorb it by working.

### 2 · How does it fit together? — *2 min*
→ [`architecture.md`](architecture.md)

Look at the diagram first, then the service table. The two things to actually retain:

- **One frontend, many independent services, one database each.** Services talk over REST. Nobody touches anybody else's database.
- **Career Memory updates passively.** Modules emit activity events as a side effect of normal use; Career Memory consumes them. That's the product's core mechanic.

### 3 · What do the words mean? — *1 min*
→ [`glossary.md`](glossary.md)

Skim it. Two traps worth catching now: "pipeline" here means the application orchestration layer, **not** CI. And *module* (product scope), *service* (deployment unit), and *phase* (time) are three different things people wrongly use interchangeably.

### 4 · How do we work? — *1 min*
→ [`conventions.md`](conventions.md)

Branch naming, commit format, PR expectations, and the ADR process. Read the commit and ADR sections properly — they're the two you'll use on day one.

---

## You're now oriented. Next:

**Picking up a module?** Read that folder's `README.md` and `AGENTS.md` — every module folder has both. Start with [`career-memory/`](../career-memory/README.md); everything else depends on it.

**About to make a decision?** Check [`decisions/`](decisions/) first — it may already be made. If it's genuinely open, it's probably in [`open-questions.md`](open-questions.md).

**Working with an AI agent?** Point it at [`../AGENTS.md`](../AGENTS.md). It's the agent-facing entry point and mirrors this one.

---

## Before your first PR

Non-negotiable, in this order:

1. [`conventions.md`](conventions.md) — commit format and PR expectations
2. [`../.agents/rules.md`](../.agents/rules.md) — the hard constraints. Written for agents, but they bind humans identically. This is the fastest way to learn what will get your PR rejected.
3. Your module's `AGENTS.md` — what your service owns and what it must not touch

## Read when you need it, not before

| Doc | Read when |
|---|---|
| [`decisions/`](decisions/) (ADRs 0001–0007) | You want to know *why* something is the way it is, or you're about to argue with it |
| [`schema/career-memory.md`](schema/career-memory.md) | You're touching Career Memory data — required reading then |
| [`spec/project-spec-v2.md`](spec/project-spec-v2.md) | You need original product intent or a feature list. **Historical** — ADRs override it |
| [`open-questions.md`](open-questions.md) | You've hit something undecided, or you're in a planning session |

---

## Current state

**Phase 0.** Contracts are being agreed. There is no code yet.

The tech stack is **not decided** — that's why no doc here tells you how to install or run anything. It's the top item in [`open-questions.md`](open-questions.md), and it blocks the most.

If something in the docs is wrong, stale, or missing, fix it in the same PR as the work that revealed it. Onboarding time only stays at 5 minutes if it's maintained.
