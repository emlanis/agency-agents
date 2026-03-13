# VS Code + GitHub Setup for Codex (Fork/Clone Workflow)

This guide walks through a practical local workflow for contributing to **`emlanis/agency-agents`** from VS Code while running Codex agent sessions inside the repository.

---

## 1) Fork + Clone Flow for `emlanis/agency-agents`

### A. Fork on GitHub
1. Go to `https://github.com/emlanis/agency-agents`.
2. Click **Fork** and create a copy under your own GitHub account.

### B. Clone your fork locally
Use either HTTPS or SSH:

```bash
# HTTPS
git clone https://github.com/<your-username>/agency-agents.git

# OR SSH
git clone git@github.com:<your-username>/agency-agents.git
```

```bash
cd agency-agents
```

### C. Add upstream remote
Keep your fork synchronized with the original repo:

```bash
git remote add upstream https://github.com/emlanis/agency-agents.git
git remote -v
```

You should see both:
- `origin` → your fork
- `upstream` → `emlanis/agency-agents`

### D. Sync with upstream before starting work

```bash
git checkout main
git fetch upstream
git rebase upstream/main
# Optional: update your fork's main branch
git push origin main --force-with-lease
```

---

## 2) Open in VS Code + Recommended Extensions/Settings

### A. Open workspace
From terminal:

```bash
code .
```

Or open the folder from VS Code: **File → Open Folder...**

### B. Recommended extensions
Install these for a smoother docs + Git workflow:
- **GitHub Pull Requests and Issues** (`GitHub.vscode-pull-request-github`)
- **GitLens — Git supercharged** (`eamodio.gitlens`)
- **EditorConfig for VS Code** (`EditorConfig.EditorConfig`)
- **Markdown All in One** (`yzhang.markdown-all-in-one`)
- **Code Spell Checker** (`streetsidesoftware.code-spell-checker`) for docs hygiene

### C. Practical workspace settings (`.vscode/settings.json` suggestion)

```json
{
  "files.eol": "\n",
  "files.insertFinalNewline": true,
  "files.trimTrailingWhitespace": true,
  "editor.formatOnSave": true,
  "markdown.validate.enabled": true,
  "git.autofetch": true,
  "git.confirmSync": false
}
```

Tip: Keep team-wide defaults minimal; use personal User Settings for anything opinionated.

---

## 3) Run Codex-Driven Agent Sessions Inside the Repo

### A. Start from repo root
Make sure your terminal is in the cloned repository:

```bash
cd /path/to/agency-agents
pwd
```

### B. Launch Codex in project context
Run Codex from this directory so it can read and modify repo files directly.

Typical session pattern:
1. Describe the task (e.g., “Add a new integration guide and update README links”).
2. Ask Codex to run checks after edits.
3. Ask Codex to stage, commit, and prepare PR text.

### C. Keep sessions focused
For iterative MVP work, prefer small prompts tied to one deliverable:
- “Draft the file skeleton first.”
- “Now fill in troubleshooting examples.”
- “Now update README cross-links.”

This keeps diffs reviewable and makes rollback easy.

---

## 4) Branch → Commit → PR Loop (Iterative MVP)

Use short-lived branches and small PRs.

### A. Create a feature branch

```bash
git checkout -b docs/vscode-github-setup
```

### B. Implement one thin slice
Example: create one guide + README links.

### C. Validate + review local diff

```bash
git status
git diff -- README.md integrations/codex/VSCODE-GITHUB-SETUP.md
```

### D. Commit with clear message

```bash
git add README.md integrations/codex/VSCODE-GITHUB-SETUP.md
git commit -m "docs: add VS Code + GitHub Codex setup guide"
```

### E. Push and open PR

```bash
git push -u origin docs/vscode-github-setup
```

Then open a PR from your fork branch into `emlanis/agency-agents:main`.

### F. Repeat in small increments
After review feedback, push follow-up commits to the same branch (or create a new branch for the next MVP slice).

---

## 5) Troubleshooting (VS Code + GitHub Auth)

### Issue: VS Code source control asks you to sign in repeatedly
**Fixes:**
- Confirm you are signed into the correct GitHub account in VS Code (**Accounts** menu).
- Re-authenticate the **GitHub Pull Requests and Issues** extension.
- Reload window: `Developer: Reload Window`.

### Issue: `git push` fails with HTTPS auth errors
**Fixes:**
- Use a GitHub Personal Access Token (PAT) instead of a password.
- Refresh credential helper cache:

```bash
git config --global credential.helper manager-core
```

On Linux where `manager-core` is unavailable, use:

```bash
git config --global credential.helper store
```

Then retry push and enter username + PAT.

### Issue: SSH push fails (`Permission denied (publickey)`)
**Fixes:**
- Check for keys: `ls ~/.ssh`
- Create key if needed:

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

- Add key to agent:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

- Add public key (`~/.ssh/id_ed25519.pub`) to GitHub SSH keys.
- Verify connection:

```bash
ssh -T git@github.com
```

### Issue: Wrong remote URL (pushing to upstream directly)
Check remotes:

```bash
git remote -v
```

Set fork as `origin` and upstream as `upstream`:

```bash
git remote set-url origin git@github.com:<your-username>/agency-agents.git
git remote set-url upstream https://github.com/emlanis/agency-agents.git
```

### Issue: Line ending noise in diffs
Use repo-safe defaults:

```bash
git config --global core.autocrlf input
```

And keep VS Code using LF for this repo.

---

If you are new to contributing, optimize for **small PRs, clean commit messages, and frequent sync with upstream**.
