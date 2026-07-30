# Unified Application Pipeline (Module 6)

The UX wrapper that connects all modules into one continuous flow. Transforms a collection of tools into a product.

## Overview
- Single entry point: search or paste a job to begin
- Linear flow: Search → JD Analysis → Procedure Guide → Tracker → Document Tailor
- Passive Career Memory updates at every step
- Dashboard: active applications, upcoming deadlines, recent activity, fit scores at a glance
- State continuity: user context (userId, jobId, applicationId) persists across modules

## Product Form
This is the orchestration layer / API Gateway in the microservices architecture — see [Repo_Structure.md](Repo_Structure.md). Per [Auth_And_UI.md](Auth_And_UI.md), its UI shell is being front-loaded into Phase 1 as the one unified frontend, rather than built only in Phase 3.

## Storage Model
N/A — orchestrates calls to other modules' APIs, does not own its own primary data (aside from UI-level state).
