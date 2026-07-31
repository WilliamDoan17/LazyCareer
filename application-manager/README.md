# application-manager

**Module 4 — Application Manager (4a + 4b).** Procedure guide and progress tracker, shipped as one module.

| | |
|---|---|
| **Kind** | REST API / microservice |
| **Phase** | 2 |
| **Complexity** | Low–Medium |
| **Status** | Empty — not started |
| **Owner** | TBD |
| **Depends on** | career-memory (writes via events) |
| **Depended on by** | pipeline, app-intelligence |

## Responsibility

Solves problems 4a and 4b together: *what do I need to do*, and *where am I in doing it*.

They ship as one module because either alone is useless — a procedure guide without a tracker is just a list, and a tracker without guidance is just a spreadsheet.

**4a — Position Procedure Guide**
- Standard procedure templates by role type (SWE, PM, design…)
- AI-generated company-specific notes where data is available
- Checklist of action items per stage

**4b — Progress Tracker**
- Kanban + table view
- Per-application task list with deadlines
- Deadline reminders and notes per application
- Status updates trigger Career Memory log entries automatically

## Application stages

Canonical, ordered:

```
Applied → OA → Phone Screen → Interview → Offer → Rejected/Ghosted
```

`Rejected/Ghosted` is terminal and covers both explicit rejection and silence.

## Owns

Applications, stages, tasks, deadlines, notes. Its own database.

## Status

Blocked on the [tech stack decision](../docs/open-questions.md). Phase 2 — starts after Career Memory is mature.

See [`AGENTS.md`](AGENTS.md) · [architecture](../docs/architecture.md)
