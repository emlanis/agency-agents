# ⚡ NEXUS Quick-Start Guide

> **Get from zero to orchestrated multi-agent pipeline in 5 minutes.**

---

## What is NEXUS?

**NEXUS** (Network of EXperts, Unified in Strategy) turns The Agency's AI specialists into a coordinated pipeline. Instead of activating agents one at a time and hoping they work together, NEXUS defines exactly who does what, when, and how quality is verified at every step.

## Choose Your Mode

| I want to... | Use | Agents | Time |
|-------------|-----|--------|------|
| Build a complete product from scratch | **NEXUS-Full** | All | 12-24 weeks |
| Build a feature or MVP | **NEXUS-Sprint** | 15-25 | 2-6 weeks |
| Do a specific task (bug fix, campaign, audit) | **NEXUS-Micro** | 5-10 | 1-5 days |

---


## Codex + VS Code Usage Pattern

Use this sequence for reliable orchestration in Codex sessions:

1. **Open the correct workspace root** in VS Code before starting Codex (so relative paths, tests, and handoffs stay consistent).
2. **Start with one orchestration prompt** (NEXUS-Full/Sprint/Micro) and require explicit deliverables per phase.
3. **Request structured handoffs** between agents using: Context → Decisions → Open Risks → Next Owner.
4. **Keep one running thread per workstream** (feature, bug, campaign) to preserve state and reduce context drift.
5. **Close each loop with evidence** (tests, screenshots, metrics) before asking Codex to advance phases.

### Codex-Oriented Prompt Templates

#### Workspace kickoff (VS Code)

```
You are operating inside this VS Code workspace.
Activate Agents Orchestrator in NEXUS-Sprint mode for: [FEATURE/PROJECT].
Use only files in this workspace as source of truth.

Output in this order:
1) Phase plan with owners
2) Immediate Task 1 implementation plan
3) First handoff in format: Context / Decisions / Risks / Next Owner
```

#### Implementation + QA loop

```
Activate Frontend Developer for task: [TASK].
After implementation, activate API Tester and Evidence Collector.
Do not mark complete until tests pass and evidence is attached.
Return a handoff block: Context / Decisions / Risks / Next Owner.
```

#### Multi-agent handoff enforcement

```
For every agent transition, produce a handoff with:
- Context received
- Work completed
- Decisions made
- Known risks / unknowns
- Next owner + exact requested action
```

## Migration Note (Claude/Cursor → Codex)

- **What stays identical:** agent markdown files, NEXUS phases, quality gates, and handoff structure all transfer directly.
- **What to rename:** replace tool-specific language like “Claude session” or “Cursor chat” with “Codex session” / “VS Code Codex chat”.
- **Where to store agent files in Codex workflows:** keep the repo in your project workspace and optionally mirror agents to `~/.codex/agents` for reusable local activation.

---

## 🚀 NEXUS-Full: Start a Complete Project

**Copy-paste prompt (Codex chat or VS Code Codex panel):**

```
Activate Agents Orchestrator in NEXUS-Full mode.

Project: [YOUR PROJECT NAME]
Specification: [DESCRIBE YOUR PROJECT OR LINK TO SPEC]

Execute the complete NEXUS pipeline:
- Phase 0: Discovery (Trend Researcher, Feedback Synthesizer, UX Researcher, Analytics Reporter, Legal Compliance Checker, Tool Evaluator)
- Phase 1: Strategy (Studio Producer, Senior Project Manager, Sprint Prioritizer, UX Architect, Brand Guardian, Backend Architect, Finance Tracker)
- Phase 2: Foundation (DevOps Automator, Frontend Developer, Backend Architect, UX Architect, Infrastructure Maintainer)
- Phase 3: Build (Dev↔QA loops — all engineering + Evidence Collector)
- Phase 4: Harden (Reality Checker, Performance Benchmarker, API Tester, Legal Compliance Checker)
- Phase 5: Launch (Growth Hacker, Content Creator, all marketing agents, DevOps Automator)
- Phase 6: Operate (Analytics Reporter, Infrastructure Maintainer, Support Responder, ongoing)

Quality gates between every phase. Evidence required for all assessments.
Maximum 3 retries per task before escalation.
```

---

## 🏃 NEXUS-Sprint: Build a Feature or MVP

**Copy-paste prompt (Codex session):**

```
Activate Agents Orchestrator in NEXUS-Sprint mode.

Feature/MVP: [DESCRIBE WHAT YOU'RE BUILDING]
Timeline: [TARGET WEEKS]
Skip Phase 0 (market already validated).

Sprint team:
- PM: Senior Project Manager, Sprint Prioritizer
- Design: UX Architect, Brand Guardian
- Engineering: Frontend Developer, Backend Architect, DevOps Automator
- QA: Evidence Collector, Reality Checker, API Tester
- Support: Analytics Reporter

Begin at Phase 1 with architecture and sprint planning.
Run Dev↔QA loops for all implementation tasks.
Reality Checker approval required before launch.
```

---

## 🎯 NEXUS-Micro: Do a Specific Task

**Pick your scenario and copy-paste into Codex:**

### Fix a Bug
```
Activate Backend Architect to investigate and fix [BUG DESCRIPTION].
After fix, activate API Tester to verify the fix.
Then activate Evidence Collector to confirm no visual regressions.
```

### Run a Marketing Campaign
```
Activate Social Media Strategist as campaign lead for [CAMPAIGN DESCRIPTION].
Team: Content Creator, Twitter Engager, Instagram Curator, Reddit Community Builder.
Brand Guardian reviews all content before publishing.
Analytics Reporter tracks performance daily.
Growth Hacker optimizes channels weekly.
```

### Conduct a Compliance Audit
```
Activate Legal Compliance Checker for comprehensive compliance audit.
Scope: [GDPR / CCPA / HIPAA / ALL]
After audit, activate Executive Summary Generator to create stakeholder report.
```

### Investigate Performance Issues
```
Activate Performance Benchmarker to diagnose performance issues.
Scope: [API response times / Page load / Database queries / All]
After diagnosis, activate Infrastructure Maintainer for optimization.
DevOps Automator deploys any infrastructure changes.
```

### Market Research
```
Activate Trend Researcher for market intelligence on [DOMAIN].
Deliverables: Competitive landscape, market sizing, trend forecast.
After research, activate Executive Summary Generator for executive brief.
```

### UX Improvement
```
Activate UX Researcher to identify usability issues in [FEATURE/PRODUCT].
After research, activate UX Architect to design improvements.
Frontend Developer implements changes.
Evidence Collector verifies improvements.
```

---

## 📁 Strategy Documents

| Document | Purpose | Location |
|----------|---------|----------|
| **Master Strategy** | Complete NEXUS doctrine | `strategy/nexus-strategy.md` |
| **Phase 0 Playbook** | Discovery & intelligence | `strategy/playbooks/phase-0-discovery.md` |
| **Phase 1 Playbook** | Strategy & architecture | `strategy/playbooks/phase-1-strategy.md` |
| **Phase 2 Playbook** | Foundation & scaffolding | `strategy/playbooks/phase-2-foundation.md` |
| **Phase 3 Playbook** | Build & iterate | `strategy/playbooks/phase-3-build.md` |
| **Phase 4 Playbook** | Quality & hardening | `strategy/playbooks/phase-4-hardening.md` |
| **Phase 5 Playbook** | Launch & growth | `strategy/playbooks/phase-5-launch.md` |
| **Phase 6 Playbook** | Operate & evolve | `strategy/playbooks/phase-6-operate.md` |
| **Activation Prompts** | Ready-to-use agent prompts | `strategy/coordination/agent-activation-prompts.md` |
| **Handoff Templates** | Standardized handoff formats | `strategy/coordination/handoff-templates.md` |
| **Startup MVP Runbook** | 4-6 week MVP build | `strategy/runbooks/scenario-startup-mvp.md` |
| **Enterprise Feature Runbook** | Enterprise feature development | `strategy/runbooks/scenario-enterprise-feature.md` |
| **Marketing Campaign Runbook** | Multi-channel campaign | `strategy/runbooks/scenario-marketing-campaign.md` |
| **Incident Response Runbook** | Production incident handling | `strategy/runbooks/scenario-incident-response.md` |

---

## 🔑 Key Concepts in 30 Seconds

1. **Quality Gates** — No phase advances without evidence-based approval
2. **Dev↔QA Loop** — Every task is built then tested; PASS to proceed, FAIL to retry (max 3)
3. **Handoffs** — Structured context transfer between agents (never start cold)
4. **Reality Checker** — Final quality authority; defaults to "NEEDS WORK"
5. **Agents Orchestrator** — Pipeline controller managing the entire flow
6. **Evidence Over Claims** — Screenshots, test results, and data — not assertions

---

## 🎭 The Agents at a Glance

```
ENGINEERING         │ DESIGN              │ MARKETING
Frontend Developer  │ UI Designer         │ Growth Hacker
Backend Architect   │ UX Researcher       │ Content Creator
Mobile App Builder  │ UX Architect        │ Twitter Engager
AI Engineer         │ Brand Guardian      │ TikTok Strategist
DevOps Automator    │ Visual Storyteller  │ Instagram Curator
Rapid Prototyper    │ Whimsy Injector     │ Reddit Community Builder
Senior Developer    │ Image Prompt Eng.   │ App Store Optimizer
                    │                     │ Social Media Strategist
────────────────────┼─────────────────────┼──────────────────────
PRODUCT             │ PROJECT MGMT        │ TESTING
Sprint Prioritizer  │ Studio Producer     │ Evidence Collector
Trend Researcher    │ Project Shepherd    │ Reality Checker
Feedback Synthesizer│ Studio Operations   │ Test Results Analyzer
                    │ Experiment Tracker  │ Performance Benchmarker
                    │ Senior Project Mgr  │ API Tester
                    │                     │ Tool Evaluator
                    │                     │ Workflow Optimizer
────────────────────┼─────────────────────┼──────────────────────
SUPPORT             │ SPATIAL             │ SPECIALIZED
Support Responder   │ XR Interface Arch.  │ Agents Orchestrator
Analytics Reporter  │ macOS Spatial/Metal │ Data Analytics Reporter
Finance Tracker     │ XR Immersive Dev    │ LSP/Index Engineer
Infra Maintainer    │ XR Cockpit Spec.    │ Sales Data Extraction
Legal Compliance    │ visionOS Spatial    │ Data Consolidation
Exec Summary Gen.   │ Terminal Integration│ Report Distribution
```

---

<div align="center">

**Start with a mode. Follow the playbook. Trust the pipeline.**

`strategy/nexus-strategy.md` — The complete doctrine

</div>
