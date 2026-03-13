# 🧭 Ambassador OS Architecture

> Purpose: Define the production architecture for social data ingestion, multi-tenant auth/isolation, paid subscription enforcement, and KPI analytics so implementation can proceed with clear sequencing.

---

## 1) Data Ingestion from Twitter/X API

### 1.1 Core entities

Persist social data with explicit ownership boundaries (`workspace_id`) and source traceability:

- `x_accounts`
  - `id` (internal UUID), `workspace_id`, `x_user_id`, `handle`, `display_name`, `profile_image_url`, `verified`, `follower_count`, `following_count`, `tweet_count`, `listed_count`, `created_at_x`, `last_synced_at`
- `x_posts`
  - `id`, `workspace_id`, `x_post_id`, `x_account_id`, `text`, `lang`, `conversation_id`, `in_reply_to_x_post_id`, `quoted_x_post_id`, `is_retweet`, `is_quote`, `posted_at`, `ingested_at`
- `x_post_metrics_daily`
  - `id`, `workspace_id`, `x_post_id`, `metric_date`, `impressions`, `likes`, `replies`, `reposts`, `bookmarks`, `profile_clicks`, `detail_expands`
- `x_mentions`
  - `id`, `workspace_id`, `x_post_id`, `mentioned_x_user_id`, `mentioned_handle`
- `x_sync_jobs`
  - `id`, `workspace_id`, `job_type` (`incremental`, `backfill`, `repair`), `status`, `cursor`, `window_start`, `window_end`, `attempt_count`, `last_error`, `started_at`, `finished_at`
- `x_rate_limit_events`
  - `id`, `workspace_id`, `endpoint`, `limit`, `remaining`, `reset_at`, `observed_at`

### 1.2 Sync schedule

Use a queue-based scheduler (e.g., Supabase cron + job table or dedicated worker scheduler) with per-workspace jobs:

- Incremental content sync: every **15 minutes**
  - Pull new posts for managed accounts and tracked query streams.
- Metrics refresh for recent posts: every **6 hours** for posts in last 7 days.
- Metrics finalization: daily refresh for posts aged 8-30 days.
- Mentions scan: every **30 minutes**.
- Account/profile snapshot refresh: daily.

All jobs should be idempotent using `(workspace_id, x_post_id)` and `(workspace_id, x_post_id, metric_date)` unique constraints.

### 1.3 Retry policy

For transient failures (timeouts, HTTP 5xx, network errors, and 429):

- Exponential backoff with jitter: `30s, 2m, 10m, 30m, 2h` (max 5 attempts per run).
- On HTTP 429:
  - Parse reset window, pause endpoint-specific consumer until `reset_at + 5s`.
  - Record into `x_rate_limit_events`.
- On permanent 4xx (except 429): mark job `failed_terminal`, attach error payload.
- Dead-letter rule: after max attempts, move job to `needs_manual_review` for operator replay.

### 1.4 Backfill strategy

Two-stage backfill to control API and storage cost:

1. **Bootstrap window** (default: last 90 days)
   - Pull posts chronologically oldest → newest to establish durable cursor and avoid missing thread context.
2. **Historical extension** (optional by plan/admin action)
   - Backfill month-by-month until configured retention boundary.

Backfill guardrails:

- One active backfill per workspace.
- Priority lower than incremental jobs.
- Auto-pause if incremental lag exceeds 30 minutes.

### 1.5 Observability + data quality

Track ingestion SLOs and integrity:

- SLO: 95% of incremental jobs finish within 10 minutes.
- Freshness metric: max(`now - latest_post.posted_at`) per workspace.
- Completeness checks:
  - No null `workspace_id` in ingested tables.
  - Metric row count parity checks vs post population for 7-day window.
- Operational dashboards: job success rate, API quota burn, duplicate upsert conflicts, lag by workspace.

---

## 2) Supabase Auth + Tenant Isolation Model

### 2.1 Identity model

Use Supabase Auth for user identity, then map users into workspace-scoped roles:

- `users` (from `auth.users`) for identity.
- `workspaces`
  - `id`, `name`, `created_by`, `stripe_customer_id`, `subscription_status`
- `workspace_memberships`
  - `workspace_id`, `user_id`, `role` (`owner`, `program_lead`, `analyst`, `viewer`), `status`

Program leads are a role in `workspace_memberships` with elevated permissions on campaign config, integrations, and reporting exports.

### 2.2 Workspace scoping

Every product table must include `workspace_id` (except global reference tables). All read/write paths must bind to an explicit active workspace resolved from membership.

Request lifecycle pattern:

1. User authenticates with Supabase JWT.
2. API resolves active workspace from request header/context.
3. Service verifies membership exists and status is active.
4. DB operations execute with RLS enforcing `workspace_id` boundary.

### 2.3 Row-level security strategy

RLS baseline:

- Enable RLS on all tenant tables.
- Deny-by-default policies.
- Policy condition pattern:
  - `workspace_id IN (SELECT workspace_id FROM workspace_memberships WHERE user_id = auth.uid() AND status = 'active')`

Role-sensitive controls:

- `viewer`: read-only on analytics/reporting tables.
- `analyst`: read/write on campaign annotations and KPI targets.
- `program_lead`: analyst permissions + integration management + exports.
- `owner`: full workspace admin, billing controls.

Implementation notes:

- Use security-definer SQL functions for sensitive write flows (e.g., membership changes) and validate caller role inside function.
- Keep Stripe webhook writes in privileged service role pathway (never client JWT).
- Add automated policy tests to verify cross-tenant access is impossible.

### 2.4 Audit and governance

Add append-only `audit_events` table:

- Capture actor (`user_id` or `service`), `workspace_id`, action type, entity, before/after snapshot references, timestamp.
- Required for membership changes, integration token updates, exports, and billing state changes.

---

## 3) Stripe Subscription Model ($200/month)

### 3.1 Product + price contract

- Stripe Product: `Ambassador OS`
- Stripe Price: recurring monthly at **$200 USD** (`unit_amount=20000`, `interval=month`)
- Billing anchor: checkout timestamp unless explicitly aligned by admin action.

### 3.2 Checkout flow

1. Authenticated workspace owner opens billing.
2. Backend creates Stripe Checkout Session with:
   - `mode=subscription`
   - `customer` (existing) or create + persist `stripe_customer_id`
   - line item: `$200/month` price id
   - metadata: `workspace_id`, `initiated_by_user_id`
3. Client redirects to hosted checkout.
4. Success/cancel URLs return to billing settings UI.

### 3.3 Webhook events and state machine

Consume Stripe webhooks via signed endpoint; persist events idempotently (`stripe_event_id` unique).

Key events:

- `checkout.session.completed`
  - Associate customer + subscription with workspace.
- `customer.subscription.created`
- `customer.subscription.updated`
- `customer.subscription.deleted`
- `invoice.paid`
- `invoice.payment_failed`

Internal subscription states:

- `trialing` (optional), `active`, `past_due`, `canceled`, `incomplete`

State transitions update:

- `workspaces.subscription_status`
- `workspaces.current_period_end`
- `workspaces.entitlement_active` (derived boolean)

### 3.4 Entitlement checks

Use server-side entitlement gate on every paid feature path:

- Required: `subscription_status IN ('active', 'trialing')`
- Grace behavior (recommended): allow read-only access for `past_due` up to 7 days, block new ingestion jobs and exports.
- Hard block for `canceled` or prolonged unpaid status.

Enforcement points:

- API middleware for paid endpoints.
- Worker guard before scheduling ingestion/report jobs.
- UI capability flags from backend entitlement endpoint (do not trust client-only checks).

### 3.5 Failure handling

- If webhook delivery fails, retry via Stripe and support manual replay endpoint by event id.
- Daily reconciliation job compares Stripe source-of-truth subscriptions against local workspace records.
- Alert when local status diverges > 1 hour.

---

## 4) Analytics/Reporting Pipeline (Replace Mock KPIs)

### 4.1 Source-to-metric flow

Replace mocked KPI providers with deterministic aggregation pipeline:

1. **Raw ingestion layer**: `x_posts`, `x_post_metrics_daily`, engagement events.
2. **Normalization layer**: standardize timezones (UTC), dedupe, enrich post/account dimensions.
3. **Metric marts**:
   - `kpi_workspace_daily`
   - `kpi_campaign_daily`
   - `kpi_content_daily`
4. **Serving layer**: SQL views or materialized views consumed by API/reporting.

### 4.2 KPI definitions (initial)

- Reach proxy: sum of impressions.
- Engagements: likes + replies + reposts + bookmarks.
- Engagement rate: engagements / impressions (guard divide-by-zero).
- Posting volume: count of posts.
- Response activity: count of replies authored by managed accounts.
- Growth: follower delta day-over-day.

Each KPI should include versioned definition metadata (`kpi_definition_version`) for change traceability.

### 4.3 Compute cadence

- Near-real-time incremental aggregates: every 15 minutes for current day.
- Daily close job at 01:00 UTC recomputes previous day as authoritative snapshot.
- 7-day rolling recompute nightly to absorb late-arriving metrics.

### 4.4 Data contracts and quality controls

- Metric contracts with explicit numerator/denominator definitions.
- Not-null and range checks (e.g., impressions >= engagements when required by platform semantics).
- Freshness SLA: dashboard data lag < 30 minutes during active subscription state.
- Reconciliation report: compare aggregate totals to raw-source rollups.

### 4.5 Reporting outputs

- API endpoints:
  - `/reporting/overview?workspace_id&date_range`
  - `/reporting/campaigns?...`
  - `/reporting/content?...`
- Export jobs (CSV/PDF) generated asynchronously and audited in `audit_events`.
- Scheduled email digests (weekly) behind entitlement gate.

---

## 5) Implementation Order

Execute in this order to minimize rework and unblock dependencies:

1. **Tenant + auth foundation**
   - Create workspace/membership schema, RLS policies, role model, policy tests.
2. **Billing foundation**
   - Stripe product/price config, checkout API, webhook ingestion, entitlement middleware.
3. **X ingestion core**
   - Account/post entities, scheduler, incremental sync, retry + rate-limit handling.
4. **Backfill + observability**
   - Backfill orchestrator, lag dashboards, dead-letter handling, replay tooling.
5. **Analytics pipeline v1**
   - KPI marts, aggregation jobs, contract tests, replace mock data source wiring.
6. **Reporting + exports**
   - Reporting APIs, async exports, weekly digest, audit coverage.
7. **Hardening**
   - Reconciliation jobs (Stripe + metrics), load tests, incident runbooks, on-call alerts.

Dependency rule: no analytics/reporting work should ship to production until tenant isolation + entitlement checks are in place.

---

## 6) Minimal Milestone Map

| Milestone | Target | Exit Criteria |
|---|---|---|
| M1: Secure Multi-Tenant Core | Week 1 | Users can authenticate, join workspace, and are isolated by verified RLS tests. |
| M2: Paid Access Control | Week 2 | $200/month checkout live in test mode; webhook-driven entitlement status gates paid endpoints. |
| M3: Reliable X Incremental Sync | Week 3 | 15-minute incremental sync stable with retries, rate-limit awareness, and per-workspace lag metrics. |
| M4: Backfill + Data Integrity | Week 4 | 90-day backfill available with pause/resume and quality checks passing on sampled workspaces. |
| M5: Real KPI Pipeline (No Mocks) | Week 5 | KPI dashboards served from production aggregates with freshness + reconciliation checks. |
| M6: Reporting Readiness | Week 6 | Overview/campaign/content reports + exports available and fully entitlement/audit enforced. |

---

## 7) Execution Notes for Engineering Agents

- Treat `workspace_id` and entitlement status as mandatory invariants in every service boundary.
- Prefer idempotent jobs and append-only event logs to simplify replay/recovery.
- Ship each milestone with explicit runbooks: failure modes, rollback, and verification commands.
- Block release when cross-tenant policy tests or billing reconciliation checks fail.
