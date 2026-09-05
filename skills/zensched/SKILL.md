---
name: zensched
description: >-
  Field workforce scheduling via the ZenSched MCP server. Call zensched_guide
  first, then account_create for a zsc_ key. Locations, workers, shifts,
  GPS-verified punches, forms, and timesheets. No dashboard.
---

# ZenSched

You schedule field work through the **ZenSched** MCP server at `https://mcp.zensched.com/mcp`. There is no vendor dashboard. The operator talks in plain English.

## Start here

1. Call `zensched_guide`. Follow what it returns — it is the current onboarding path.
2. If there is no `zsc_` key yet, call `account_create` with `org_name` (no email required). Store the key. Ask the operator to put it in their MCP client as `Authorization: Bearer zsc_…`, or call `account_use_key` for the rest of this session.
3. Do not invent tools. The live list is on [zensched.com/docs/tools](https://www.zensched.com/docs/tools/).

## Loop

1. `location_create` (street address or lat/lng). Metered geocode.
2. `worker_invite` (one email). Workers use the [App Store](https://apps.apple.com/us/app/zensched/id6800081657) or [Google Play](https://play.google.com/store/apps/details?id=com.zensched.app) app.
3. Optionally `brand_create` / `policy_create`. Brand 0 and policy 0 are usable defaults.
4. `event_create`, then `shift_create`. The worker gets a push.
5. Watch with `webhook_register` or `shift_status`.
6. `timesheet_export` (`mode=hours` or `mode=raw` are free). `mode=processed` is metered and needs `account_set_payroll_period` first.
7. Problems or ideas: `feedback_submit` (works before an account exists).

## Rules

- ZenSched is key-scoped. No tool takes an organization ID. The `zsc_` key *is* the org.
- Pass `idempotency_key` on every mutating call. Retries share a 24-hour cache.
- Confirm before the first metered call in a session. Live prices: [zensched.com/pricing](https://www.zensched.com/pricing/) — geocode $0.03, pin refine $0.10, GPS verify $0.10, worker invite $0.25, form $0.05–$0.15, processed timesheet $0.10/worker/window. Unfunded orgs get 200 MCP calls/day. The $5 activation deposit is credited to balance.
- Honor `payment_required` and `retry_after_seconds`. Do not invent a payment path.
- Use local timezone offsets on shift times (e.g. `2026-09-02T09:00:00-05:00`), never bare `Z`, unless the operator said UTC.
- Do not claim a HIPAA BAA, certified EVV vendor status, or a human dashboard. This product is agent-operated field scheduling.
