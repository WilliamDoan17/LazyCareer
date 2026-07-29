# 0001 — Career Memory Product Form

## Decision
Career Memory (Module 1) ships as a **REST API / microservice**, with a thin web UI for onboarding, manual edit, and export. It is not a standalone web app, not an MCP server, and not a pure skill/prompt layer.

## Why
Other modules — JD Analysis, Application Manager, Career Document Tailor — read and write Career Memory data programmatically. It needs persistent structured storage and a stable API contract other services can call, matching the project spec's Phase 0 requirement for a "Shared API Contract."

## Consequence
`career-memory/` exposes REST endpoints as its primary interface. The UI is a secondary consumer of that same API, not a separate product.
