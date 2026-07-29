# 0002 — Storage Model

## Decision
Career Memory data is stored as a **relational DB schema per user** (Postgres tables: `users`, `skills`, `experience`, `projects`, `education`, `certifications`, `activity_log`), not as a per-user file (JSON/Markdown blob).

## Why
Other modules need atomic partial updates — add one skill, log one activity event — without a read-modify-write race on a single blob. A relational schema also gives queryability (e.g. join skills to the experience that used them) that a flat file can't.

## Consequence
Markdown/JSON export is a **generated output** (a feature: "export profile as Markdown or JSON"), not the source of truth. The DB is authoritative; exports are derived on request.
