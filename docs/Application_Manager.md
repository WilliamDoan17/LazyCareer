# Application Manager (Module 4 — 4a + 4b)

Ships 4a (Position Procedure Guide) and 4b (Progress Tracker) as a single cohesive module. Together they answer: what do I need to do, and where am I in doing it? They ship together because they're inseparable from the user's perspective — a procedure guide without a tracker is just a list, and a tracker without guidance is just a spreadsheet.

## Overview
- Standard procedure templates by role type (SWE, PM, design, etc.)
- AI-generated company-specific notes where data is available
- Checklist of action items per stage (tailor resume, prep for OA, research company…)
- Kanban + table view: Applied → OA → Phone Screen → Interview → Offer → Rejected/Ghosted
- Per-application task list with deadlines
- Status updates trigger Career Memory log entries automatically
- Deadline reminders and notes per application

## Product Form
TBD — needs a dedicated design session. Likely REST API/microservice; status changes emit events consumed by [Career_Memory.md](Career_Memory.md)'s `activity_log`, per [API_Contracts.md](API_Contracts.md)'s event bus convention.

## Storage Model
TBD.

## Schema
TBD.
