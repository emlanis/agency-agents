# Ambassador OS Architecture Blueprint

## Objective

Define a production MVP architecture for Ambassador Program OS using Twitter/X data, Supabase multi-tenant auth, and Stripe billing.

## Core domains

- **Identity & tenancy**: program leads, workspaces, memberships.
- **Ambassador performance**: ambassadors, social accounts, KPI snapshots.
- **Reporting**: weekly summaries, AI-generated recommendations.
- **Billing**: subscription status and workspace entitlements.

## Data flow

1. Program lead authenticates via Supabase.
2. Workspace-scoped services query ambassadors and KPI snapshots.
3. X ingestion worker fetches post/activity metrics and writes normalized snapshots.
4. Dashboard and Ambassador tabs render only workspace-scoped records.
5. AI Reports reads workspace metrics and generates executive summary.

## Supabase model (MVP)

- `workspaces`
- `workspace_members`
- `ambassadors`
- `ambassador_social_accounts`
- `kpi_snapshots`
- `weekly_reports`
- `subscriptions`

Use row-level security to enforce workspace isolation on all tenant tables.

## Twitter/X ingestion

- Service account credentials stored in secure environment variables.
- Pull schedule: every 6-12 hours for active ambassadors.
- Backfill path: manual trigger per ambassador.
- Reliability: retries, rate-limit handling, and ingestion logs.

## Stripe billing ($200/month)

- Single paid plan: `ambassador_os_monthly_200`.
- Checkout creates/links Stripe customer to workspace.
- Webhooks handled:
  - `checkout.session.completed`
  - `invoice.paid`
  - `invoice.payment_failed`
  - `customer.subscription.deleted`
- Entitlement check middleware gates paid routes/features.

## Implementation order

1. Supabase auth + workspace model + RLS.
2. KPI schema and ingestion job for one ambassador handle.
3. Dashboard KPI cards on real persisted data.
4. Stripe checkout + webhook sync.
5. Ambassador drilldown + AI weekly report from real workspace data.

## MVP non-goals

- Multi-plan pricing tiers.
- Full historical migration from third-party CSVs.
- Advanced attribution modeling beyond initial referral/event tracking.
