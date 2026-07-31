# shared — agent rules

Read [`/AGENTS.md`](../AGENTS.md) first. This adds rules specific to `shared/`.

## What this owns

The single definition of every shape that crosses a service boundary: types, validators, the error envelope, auth helpers, UI kit.

## Hard rules

- **Zero repo-internal dependencies.** `shared/` imports from no service, ever. If you need something from a service here, the design is wrong — stop and ask.
- **Additive by default.** Every service imports from here; a breaking change breaks all of them at once. Removing or retyping an exported shape requires asking first.
- **The error envelope is defined here and nowhere else.** If you see a local error shape in a service, that's a bug to report.
- **No business logic.** Shapes, validation, and helpers only. Logic belongs in the service that owns the data.

## Before changing anything

A `shared/` change is a cross-team change. Flag it in the PR description and expect closer review — see [`/.agents/boundaries.md`](../.agents/boundaries.md).

## Not decided yet

Package format, language, and build tooling are all downstream of the [tech stack decision](../docs/open-questions.md). Don't pick one by creating a manifest.
