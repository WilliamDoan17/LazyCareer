# Job Search & Filter (Module 3)

Lets users find roles matching their criteria without leaving the app. Supports both API-based discovery and manual input.

## Overview
- Filter by role, location, experience level, salary, company size, remote/hybrid
- API-based results (Adzuna, JSearch, or similar) for discovery
- Manual input: paste URL or fill a job entry form (fallback & power-user path)
- Each result links directly into JD Analysis
- Save and bookmark roles

Scraping job boards is legally grey and technically fragile. The dual approach (API + manual) ensures the module is useful from day one regardless of API coverage.

## Product Form
TBD — needs a dedicated design session. Likely REST API/microservice consumed by the unified frontend, following [API_Contracts.md](API_Contracts.md) and [Auth_And_UI.md](Auth_And_UI.md).

## Storage Model
TBD.

## Schema
TBD.
