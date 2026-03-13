# Codex Integration

Use The Agency agents with Codex in VS Code.

## Install

```bash
./scripts/install.sh --tool codex
```

This installs agent files into:

- `$CODEX_HOME/agents/agency-agents/` (if `CODEX_HOME` is set)
- `~/.codex/agents/agency-agents/` (default)

## Activate Agents in Codex

In your Codex chat, call specialists directly:

```text
Activate Frontend Developer, Backend Architect, Rapid Prototyper, Growth Hacker, and Reality Checker.
Project: Ambassador Program OS MVP.
```

## Recommended workflow

1. Run a NEXUS-Sprint kickoff prompt from `strategy/QUICKSTART.md`.
2. Keep one working handoff file in your repo and paste outputs between specialists.
3. Use Reality Checker as the release gate before launch.

For end-to-end GitHub + VS Code setup, see [VSCODE-GITHUB-SETUP.md](./VSCODE-GITHUB-SETUP.md).
