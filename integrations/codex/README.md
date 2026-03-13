# Codex Integration

Use The Agency agents with Codex in two common locations:

- **Global Codex agents**: `~/.codex/agents/`
- **VS Code project agents**: `<project>/.vscode/agents/`

The installer supports both destinations.

## Install

From this repository:

```bash
./scripts/install.sh --tool codex
```

What this does:

1. Copies all frontmatter-based agent markdown files into `~/.codex/agents/`.
2. If your current directory contains `.vscode/`, also copies the same files into `.vscode/agents/`.

> For project-scoped VS Code setup, run the installer from your project root.

## Expected Paths

After installation, you should see agent files in one or both locations:

- `~/.codex/agents/*.md`
- `<project>/.vscode/agents/*.md`

## Activating Agents in a Codex Session

During a Codex session, activate an agent by naming it in your prompt. Example:

- `Use the Frontend Developer agent to implement this component.`
- `Switch to the Security Engineer agent and audit this auth flow.`

If multiple agent files are available, Codex will use the instructions from the named agent file as session guidance.
