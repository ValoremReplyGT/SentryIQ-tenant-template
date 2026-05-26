# SentryIQ Tenant Template

> **Don't clone this repo directly.**
>
> Use the **SentryIQ Builder** to create a new tenant from this template, or
> follow [`docs/ADDING_A_TENANT.md`](https://github.com/ValoremReplyGT/SentryIQ/blob/main/docs/ADDING_A_TENANT.md)
> in the [`SentryIQ`](https://github.com/ValoremReplyGT/SentryIQ) repo.

## What this template gives you

A tenant repo skeleton that:

1. Pulls [`SentryIQ`](https://github.com/ValoremReplyGT/SentryIQ) as a git
   submodule, pinned to the `core-v1.0.0` tag.
2. Ships an empty `overlay/` tree matching the
   [Tenant Contract](https://github.com/ValoremReplyGT/SentryIQ/blob/main/docs/TENANT_CONTRACT.md).
3. Has a `.github/workflows/build-tenant.yml` workflow that builds the
   `core/frontend`, builds the `core/api-dotnet`, and lints the overlay SQL.

## Overlay you must fill in

```
overlay/
  branding.json              # palette, logo path, suggested prompts
  kpis.json                  # canonical KPI vocabulary
  objectives.json            # business objectives
  prompts/
    orchestrator.md          # orchestrator agent system prompt
    sql.md                   # SQL agent system prompt
    widget.md                # widget agent system prompt
    insights.md              # insights agent system prompt
    hypothesis.md            # hypothesis manager agent system prompt
    fabric.md                # fabric semantic agent system prompt
  openapi-overlays.json      # examples + enums merged into core/openapi specs
  fabric-data-agent-prompt.md
  seed/
    12_create_industry_schema.sql
    13_seed_industry_data.sql
    14_generate_industry_telemetry.sql
  .env.production            # tenant-specific backend URLs (NOT secrets)
```

Every file starts with a `TODO` marker. The SentryIQ Builder Agent generates
all of them automatically from a free-form industry description (Phase 3 of
the platform plan). If you're filling them by hand, see the
[Tenant Contract](https://github.com/ValoremReplyGT/SentryIQ/blob/main/docs/TENANT_CONTRACT.md)
for the schema and examples.

## CI

`build-tenant.yml` runs on every push:

- Recursively checks out the SentryIQ submodule (pinned to `core-v1.0.0`).
- Builds `core/frontend` with `VITE_TENANT_BRANDING_JSON` pointed at the
  tenant's `overlay/branding.json`.
- Builds `core/api-dotnet`.
- Lints every SQL file under `overlay/seed/`.

A green CI run is the prerequisite for the tenant deploy.

## Updating the core pin

When the SentryIQ team releases a new `core-vX.Y.Z` tag, bump the submodule
pointer:

```bash
cd SentryIQ
git fetch origin
git checkout core-v1.1.0     # (or whichever new tag)
cd ..
git add SentryIQ
git commit -m "chore: bump SentryIQ core to v1.1.0"
git push
```
