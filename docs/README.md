# LazyCareer Docs

## Build Plan
- [Backlog](Backlog.md) — ordered, thinnest-slice-first build plan; start here

## Products (modules)
1. [Career Memory](Career_Memory.md) — ✓ Core, Phase 1
2. [JD Analysis & Consultation](JD_Analysis.md) — ✓ Core, Phase 1
3. [Job Search & Filter](Job_Search.md) — ✓ Core, Phase 1
4. [Application Manager](Application_Manager.md) (4a + 4b) — ✓ Core, Phase 2
5. [Career Document Tailor](Document_Tailor.md) — ✓ Core, Phase 2
6. [Unified Application Pipeline](Pipeline.md) — ✓ Core, Phase 3
7. [Application Intelligence & Skill Gap Analyzer](App_Intelligence.md) — V2 Stretch, Phase 4
8. [Market Intelligence & Career Strategy](Market_Intelligence.md) — V2 Stretch, Phase 4

## Project aspects (cross-cutting)
- [API Contracts](API_Contracts.md) — REST conventions, versioning, error handling, idempotency, ID format, shared types
- [Event Schema](Event_Schema.md) — activity event envelope + known event types
- [Auth & UI Architecture](Auth_And_UI.md) — unified frontend, single JWT, shared auth provider
- [Repo Structure](Repo_Structure.md) — monorepo/workspaces layout, deployment
- [Env & Config](Env_Config.md) — per-service `.env` convention, Career Memory's required vars
