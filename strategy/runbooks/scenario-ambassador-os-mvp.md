# 🧭 Runbook: Ambassador OS MVP

```yaml
scenario: Build and launch an MVP of Ambassador OS in 6 weeks
primary_goals:
  - Validate core ambassador workflow with pilot users
  - Ship a production-ready MVP with measurable activation and retention
  - Establish a fast learning loop across product, engineering, and growth
```

## Scenario

You are shipping **Ambassador OS MVP** with exactly five primary actors:

- **Frontend Developer**
- **Backend Architect**
- **Growth Hacker**
- **Rapid Prototyper**
- **Reality Checker**

The team works in weekly milestones. Every milestone requires a **Reality Checker acceptance gate** before the next week begins.

## Role Charter

### Frontend Developer
- Owns UI architecture, design system usage, accessibility, and client-side performance.
- Produces production-ready UI for ambassador onboarding, campaign/task management, and performance dashboards.

### Backend Architect
- Owns API design, data model integrity, auth, observability, and deployment reliability.
- Produces stable services for user/org management, referral tracking, reward logic, and analytics pipelines.

### Growth Hacker
- Owns experiment backlog, acquisition channels, activation loops, and KPI instrumentation.
- Produces test plans, campaign assets, and growth insights tied to revenue/retention outcomes.

### Rapid Prototyper
- Owns speed of learning through prototypes, clickable flows, throwaway experiments, and hypothesis validation.
- Produces low-cost prototypes that reduce implementation risk before engineering commitment.

### Reality Checker
- Owns milestone gatekeeping, evidence quality, and risk control.
- Produces go/no-go decisions, remediation actions, and decision logs with explicit criteria.

## Week-by-Week Execution Plan

### Week 1 — Problem Framing, Scope Lock, and MVP Blueprint

**Deliverables by role**
- **Rapid Prototyper**
  - 2-3 clickable user-flow prototypes (ambassador signup, referral share, reward claim).
  - Hypothesis sheet (problem, assumption, signal, test method).
- **Growth Hacker**
  - ICP definition + channel thesis (where ambassadors are recruited first).
  - KPI tree: North Star, activation metric, and week-2 acquisition targets.
- **Backend Architect**
  - v1 architecture diagram (services, DB, event flow, integrations).
  - Draft API contract for core entities (ambassador, campaign, referral, reward).
- **Frontend Developer**
  - UI skeleton and route map aligned to prototype flows.
  - Component inventory with build-vs-reuse decisions.

**Handoffs (explicit I/O)**
- Rapid Prototyper → Frontend Developer
  - **Input:** Clickable prototypes + annotated edge cases.
  - **Output:** Implementable UI route map and interaction specs.
- Growth Hacker → Backend Architect
  - **Input:** KPI instrumentation requirements.
  - **Output:** Event schema and analytics tracking plan.
- Backend Architect → Frontend Developer
  - **Input:** API contract draft + sample payloads.
  - **Output:** Frontend data-binding plan and mock integration stubs.

**Reality Checker Gate (must pass before Week 2)**
- Scope limited to one critical journey end-to-end.
- Each KPI has an instrumentation owner and capture point.
- Architecture supports launch traffic assumptions with clear tradeoffs.
- Open risk register exists with top 5 launch risks.

---

### Week 2 — Foundation Build (Auth, Data, Core UI, Tracking)

**Deliverables by role**
- **Backend Architect**
  - Auth + role model implemented.
  - Core DB schema migrations and seed data.
  - Initial endpoints for signup, campaign assignment, referral creation.
- **Frontend Developer**
  - Functional onboarding + ambassador home screen.
  - Form validation, loading/error states, and telemetry hooks.
- **Growth Hacker**
  - Tracking implementation spec validated in staging.
  - Landing-page copy test draft and acquisition creative v1.
- **Rapid Prototyper**
  - Fast test for one risky behavior (e.g., referral share friction).
  - Recommendation memo on UX shortcut opportunities.

**Handoffs (explicit I/O)**
- Backend Architect → Frontend Developer
  - **Input:** Running endpoints, auth flow docs, sample tokens.
  - **Output:** Integrated onboarding + home flow using live APIs.
- Frontend Developer → Growth Hacker
  - **Input:** Instrumented UI events and funnel stage mapping.
  - **Output:** Event QA report and dashboard configuration.
- Rapid Prototyper → Backend Architect + Frontend Developer
  - **Input:** Prototype findings on friction points.
  - **Output:** Prioritized implementation changes for Week 3.

**Reality Checker Gate (must pass before Week 3)**
- A new ambassador can complete onboarding in staging without manual DB edits.
- Event data lands correctly for signup and first referral attempt.
- Failure states are visible and recoverable in core flows.
- No unresolved P0/P1 security issues in auth and session handling.

---

### Week 3 — Core Loop Completion (Referral → Attribution → Reward)

**Deliverables by role**
- **Backend Architect**
  - Referral attribution logic and idempotent reward-calculation service.
  - Admin endpoints for campaign and reward rule management.
- **Frontend Developer**
  - Referral creation/share UI and reward status panel.
  - Basic admin controls for campaign visibility/configuration.
- **Growth Hacker**
  - First acquisition experiment live (single channel).
  - Activation nudge assets (email/in-app copy) tied to funnel drop-offs.
- **Rapid Prototyper**
  - Experiment concept prototypes for increasing referral conversion.
  - Decision brief: which variant should be productionized in Week 4.

**Handoffs (explicit I/O)**
- Backend Architect → Growth Hacker
  - **Input:** Referral attribution event stream + reward outcome events.
  - **Output:** Experiment cohort definitions and baseline funnel benchmarks.
- Growth Hacker → Frontend Developer
  - **Input:** Copy and experiment variant requirements.
  - **Output:** Variant-ready UI placements and toggle hooks.
- Rapid Prototyper → Growth Hacker
  - **Input:** Prototype-based behavior insights.
  - **Output:** Ranked experiment backlog with confidence scores.

**Reality Checker Gate (must pass before Week 4)**
- End-to-end loop works: referral submitted, attributed, and reward status updated.
- Attribution logic demonstrates deterministic behavior for duplicate/retry scenarios.
- Experiment setup has valid control/variant separation and success metric definition.
- Admin actions are auditable for key configuration changes.

---

### Week 4 — Hardening, QA, and MVP Pilot Readiness

**Deliverables by role**
- **Frontend Developer**
  - Accessibility pass on core journeys.
  - UI performance improvements for key screens.
- **Backend Architect**
  - Observability baseline (logs, traces, alerting thresholds).
  - Rate limiting and abuse-prevention controls for referral endpoints.
- **Growth Hacker**
  - Pilot launch plan: invite sequence, conversion checkpoints, reporting cadence.
  - Messaging playbook for ambassador activation and re-engagement.
- **Rapid Prototyper**
  - “What-if” pilot support prototypes (edge-path UX, fallback interaction models).
  - Quick usability sessions summary with top fixes.

**Handoffs (explicit I/O)**
- Growth Hacker → Frontend Developer
  - **Input:** Pilot messaging and conversion checkpoints.
  - **Output:** In-product prompts/placements aligned to pilot funnel.
- Backend Architect → Reality Checker
  - **Input:** Reliability evidence (error budgets, alerts, recovery runbook).
  - **Output:** Operational readiness signoff notes or remediation list.
- Rapid Prototyper → All roles
  - **Input:** Usability findings and proposed rapid fixes.
  - **Output:** Final polish backlog for Week 5 launch.

**Reality Checker Gate (must pass before Week 5)**
- Core flows meet predefined success/error-rate thresholds in staging.
- Pilot runbook exists for support, rollback, and incident escalation.
- Known limitations are documented with owner + mitigation timeline.
- Team can demonstrate monitoring detects and explains top failure modes.

---

### Week 5 — Pilot Launch and Daily Optimization

**Deliverables by role**
- **Growth Hacker**
  - Pilot launched to first ambassador cohort.
  - Daily KPI digest (acquisition, activation, referral completion, reward claim rate).
- **Frontend Developer**
  - Fast UI iterations for highest-friction pilot steps.
  - In-app guidance updates based on live behavior.
- **Backend Architect**
  - Stability hotfixes and scale tuning from pilot traffic.
  - Data-quality checks for attribution and reward accuracy.
- **Rapid Prototyper**
  - 24-48 hour concept tests for emerging behavior patterns.
  - Recommended “fast-follow” concepts for Week 6 packaging.

**Handoffs (explicit I/O)**
- Growth Hacker → Backend Architect
  - **Input:** KPI anomalies and funnel leak diagnostics.
  - **Output:** Targeted query/log outputs confirming root causes.
- Backend Architect → Frontend Developer
  - **Input:** Prioritized defect/performance bottlenecks by user impact.
  - **Output:** UI remediation releases and validation notes.
- Rapid Prototyper → Growth Hacker + Frontend Developer
  - **Input:** Short-cycle concept test outcomes.
  - **Output:** Adopt/reject decisions for immediate optimizations.

**Reality Checker Gate (must pass before Week 6)**
- Pilot metrics are trustworthy (instrumentation drift < agreed threshold).
- No unresolved defects blocking referral or reward completion for pilot users.
- At least one optimization change shows measurable positive movement.
- Team has clear criteria for MVP “ready to expand” decision.

---

### Week 6 — MVP Decision, Documentation, and Scale Plan

**Deliverables by role**
- **Growth Hacker**
  - Pilot retrospective with channel ROI and cohort behavior analysis.
  - Expansion strategy for next cohort size and channel mix.
- **Frontend Developer**
  - MVP UI stabilization branch and prioritized UX debt list.
  - Component-level documentation for future feature velocity.
- **Backend Architect**
  - Technical debt ledger + scale roadmap (data, infra, reliability).
  - API/versioning guidelines for post-MVP feature growth.
- **Rapid Prototyper**
  - Next-wave concept prototypes (automation, gamification, ambassador segmentation).
  - Validation roadmap for post-MVP hypotheses.
- **Reality Checker**
  - Final go/no-go recommendation: **Expand**, **Constrain**, or **Rework**.
  - Decision packet with evidence, unresolved risks, and required safeguards.

**Handoffs (explicit I/O)**
- All roles → Reality Checker
  - **Input:** Final reports, metrics, debt/risk lists, and roadmap proposals.
  - **Output:** Consolidated decision packet for leadership.
- Reality Checker → All roles
  - **Input:** Go/no-go determination and rationale.
  - **Output:** 30/60/90-day execution constraints and milestone targets.

**Reality Checker Final Acceptance Gate**
- MVP achieved baseline targets for activation and referral completion.
- Reliability and data integrity are sufficient for controlled expansion.
- Top post-MVP risks have assigned owners and mitigation start dates.
- Decision outcome is documented with objective evidence and next milestones.

## Milestone Artifacts Checklist

- Weekly demo recording or walkthrough notes.
- KPI snapshot with trend deltas and interpretation.
- Updated risk register with owner/status.
- Handoff log (inputs/outputs completed, pending, blocked).
- Reality Checker gate decision and remediation actions.

## Operating Cadence

- **Daily (15 min):** role standup focused on blockers and handoffs.
- **Twice weekly (30 min):** experiment/results review led by Growth Hacker + Rapid Prototyper.
- **Weekly (45 min):** Reality Checker gate review and milestone decision.

## Definition of Done (MVP)

Ambassador OS MVP is done when an invited ambassador can:
1. Sign up and onboard,
2. Receive or join a campaign,
3. Share a referral link,
4. Generate an attributable referral event,
5. See reward status update,
6. Complete the loop with observable metrics and supportable operations.
