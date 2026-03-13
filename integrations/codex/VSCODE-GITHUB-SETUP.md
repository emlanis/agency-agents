# Codex + VS Code + GitHub Setup Guide

This guide helps you run The Agency from VS Code with Codex and your GitHub repo.

## 1) Clone and open the repository

```bash
git clone git@github.com:emlanis/agency-agents.git
cd agency-agents
code .
```

## 2) Install agents for Codex

```bash
./scripts/install.sh --tool codex
```

Verify:

```bash
ls -la "${CODEX_HOME:-$HOME/.codex}/agents/agency-agents"
```

## 3) Start your first Ambassador OS session

Use this kickoff prompt in Codex:

```text
Activate Agents Orchestrator in NEXUS-Sprint mode.

Feature/MVP: Ambassador Program OS MVP (Web3)
Timeline: 4-6 weeks
Core stack: React + Supabase + Stripe + Twitter/X ingestion

Sprint team:
- Engineering: Frontend Developer, Backend Architect, Rapid Prototyper
- Growth: Growth Hacker
- QA Gate: Reality Checker

Begin at Phase 1 with architecture and sprint plan.
```

## 4) Daily branch/commit loop in VS Code

```bash
git checkout -b feat/ambassador-os-slice-1
# make changes
git add .
git commit -m "feat: implement ambassador os slice 1"
git push -u origin feat/ambassador-os-slice-1
```

Then open a PR in GitHub.

## 5) Troubleshooting

- **Codex path not found**: ensure `CODEX_HOME` is set or use default `~/.codex`.
- **No agents detected**: re-run `./scripts/install.sh --tool codex`.
- **Wrong repo on GitHub**: confirm `git remote -v` points to `emlanis/agency-agents`.
