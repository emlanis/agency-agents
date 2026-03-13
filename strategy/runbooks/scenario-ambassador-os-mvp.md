# 🚀 Runbook: Ambassador Program OS MVP (Scenario 1)

> **Mode**: NEXUS-Sprint | **Duration**: 4-6 weeks | **Core Team**: 5 specialists

## Scenario

Build a production MVP for a Web3 Ambassador Program SaaS.

Core team:
- 🎨 Frontend Developer
- 🏗️ Backend Architect
- 🚀 Growth Hacker
- ⚡ Rapid Prototyper
- 🔍 Reality Checker

## MVP Goals

1. Replace mock KPI data with live Twitter/X ingestion.
2. Add Supabase auth and tenant-isolated workspaces for program leads.
3. Enable Stripe billing at `$200/month`.
4. Keep Dashboard, Ambassadors, and AI Reports tabs production-ready.

## Week-by-week plan

### Week 1 — Architecture + Foundations

- Backend Architect: define data model (workspace, ambassador, post metrics, snapshots).
- Frontend Developer: production UI shell and auth-aware routing.
- Rapid Prototyper: bootstrap end-to-end skeleton for fast iteration.
- **Reality Checker Gate**: architecture sign-off before implementation.

### Week 2 — Core Data & Auth

- Backend Architect: implement X ingestion service + scheduled sync.
- Frontend Developer: connect dashboard KPIs to persisted data.
- Rapid Prototyper: accelerate scaffolding for ambassador detail views.
- **Reality Checker Gate**: verify one full vertical slice (auth -> ingest -> KPI render).

### Week 3 — Billing + Reports

- Backend Architect: Stripe Checkout + webhook handling + entitlement checks.
- Frontend Developer: billing page and plan state in workspace settings.
- Rapid Prototyper: wire AI report generation to real workspace metrics.
- **Reality Checker Gate**: billing lifecycle validated in test mode.

### Week 4 — Launch Prep

- Growth Hacker: launch assets, waitlist/onboarding funnel, acquisition channels.
- Frontend Developer + Backend Architect: reliability, observability, and bug fixes.
- **Reality Checker Gate**: GO/NO-GO decision with evidence.

## Required handoffs

- Backend -> Frontend: API contracts + typed response examples.
- Frontend -> Reality Checker: URLs, test users, acceptance checklist.
- Growth Hacker -> Team: launch timeline + KPI targets.

## Exit criteria

- Live workspace login and tenant isolation.
- Real X-driven KPIs visible in Dashboard and Ambassador drilldown.
- Active Stripe subscription required for paid workspace access.
- Reality Checker signs off with release evidence.
