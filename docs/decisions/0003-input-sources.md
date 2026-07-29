# 0003 — Career Memory Input Sources

## Decision
Career Memory listens for five input sources:

1. **User input via chat** — conversational onboarding/update interface, not a static form. Parses freeform input, pre-fills structured entries, asks targeted follow-up questions only for gaps.
2. **Resume files** — PDF, DOCX, TeX, etc. Parsed into structured profile entries.
3. **Logging files** — PDF/DOCX/TeX documents describing a project or career progress. Parsed and appended to the profile/activity log.
4. **GitHub (live)** — OAuth connect once; fetch full history on first import, then live updates via webhooks/polling (projects, languages/skills, activity).
5. **LinkedIn (manual sync)** — fetch profile once via URL paste at import; re-sync only when the user clicks a manual "Update" button. **Not** continuously scraped.

Additionally, other LazyCareer modules push passive events (e.g. application status change, procedure step completed) into `activity_log` automatically.

## Why
- Chat-driven intake handles messy/partial input (most users don't have a clean resume ready) better than a rigid form, and reuses the same interface for ongoing updates.
- GitHub has an official API suitable for continuous polling/webhooks — safe to keep live.
- LinkedIn has **no public API for activity/profile-change polling**. Continuous scraping risks ToS violations and account bans — the same risk class the project spec already flags for job-board scraping. Manual, user-triggered sync avoids that risk entirely.

## Consequence
GitHub sync runs as a background job per connected user. LinkedIn sync only runs on explicit user action (import or "Update" button) — never on a timer.
