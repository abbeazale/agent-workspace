# Agent Workspace Template Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a clean public template repo in `~/Documents/agent-workspace` that keeps the reusable cross-agent workspace structure, excludes private workspace content, and is ready to publish to GitHub.

**Architecture:** Create a brand-new repo directory instead of mutating `/Users/abbe/lab`. Copy only the reusable root instructions, portable skills, `agent-setup` notes, and a sample project scaffold; then add a template-focused `README.md`, a safe `.gitignore`, and empty placeholder directories for local-only data like session dumps.

**Tech Stack:** Markdown, git, shell copy commands, `.gitignore`

---

## File Map

- Create: `/Users/abbe/Documents/agent-workspace/`
- Create: `/Users/abbe/Documents/agent-workspace/AGENTS.md`
- Create: `/Users/abbe/Documents/agent-workspace/CLAUDE.md`
- Create: `/Users/abbe/Documents/agent-workspace/README.md`
- Create: `/Users/abbe/Documents/agent-workspace/.gitignore`
- Create: `/Users/abbe/Documents/agent-workspace/.agents/skills/`
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/AGENTS.md`
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/CLAUDE.md`
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/ledger.md`
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/README.md`
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/docs/plan.md`
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/docs/resources.md`
- Create: `/Users/abbe/Documents/agent-workspace/notes/vault/sessions/.gitkeep`
- Create: `/Users/abbe/Documents/agent-workspace/notes/vault/logs/.gitkeep`
- Create: `/Users/abbe/Documents/agent-workspace/notes/vault/projects/.gitkeep`
- Create: `/Users/abbe/Documents/agent-workspace/notes/vault/projects/agent-setup/`

### Task 1: Create the clean export directory and copy the reusable base

**Files:**
- Create: `/Users/abbe/Documents/agent-workspace/`
- Create: `/Users/abbe/Documents/agent-workspace/.agents/skills/`
- Create: `/Users/abbe/Documents/agent-workspace/notes/vault/projects/agent-setup/`

- [ ] **Step 1: Verify the parent directory exists and inspect whether the target already exists**

Run:

```bash
ls "/Users/abbe/Documents" && python3 - <<'PY'
from pathlib import Path
target = Path('/Users/abbe/Documents/agent-workspace')
print('target exists:', target.exists())
PY
```

Expected: `/Users/abbe/Documents` lists successfully and the script prints whether `agent-workspace` already exists.

- [ ] **Step 2: Create the clean export directory structure**

Run:

```bash
mkdir -p "/Users/abbe/Documents/agent-workspace/.agents" && mkdir -p "/Users/abbe/Documents/agent-workspace/notes/vault/projects/agent-setup" && mkdir -p "/Users/abbe/Documents/agent-workspace/projects/example-project/docs"
```

Expected: command exits successfully with the target directories created.

- [ ] **Step 3: Copy the reusable root instruction files and portable skills**

Run:

```bash
cp "/Users/abbe/lab/AGENTS.md" "/Users/abbe/Documents/agent-workspace/AGENTS.md" && cp "/Users/abbe/lab/CLAUDE.md" "/Users/abbe/Documents/agent-workspace/CLAUDE.md" && cp -R "/Users/abbe/lab/.agents/skills" "/Users/abbe/Documents/agent-workspace/.agents/skills"
```

Expected: the root instruction files and `.agents/skills/` appear in the export repo.

- [ ] **Step 4: Copy only the public `agent-setup` notes into the export repo**

Run:

```bash
cp "/Users/abbe/lab/notes/vault/projects/agent-setup/2026-03-29-phase-2-memory-setup-design.md" "/Users/abbe/Documents/agent-workspace/notes/vault/projects/agent-setup/" && cp "/Users/abbe/lab/notes/vault/projects/agent-setup/2026-03-29-phase-2-memory-setup-plan.md" "/Users/abbe/Documents/agent-workspace/notes/vault/projects/agent-setup/" && cp "/Users/abbe/lab/notes/vault/projects/agent-setup/2026-03-30-equilibrium-work-laptop-setup.md" "/Users/abbe/Documents/agent-workspace/notes/vault/projects/agent-setup/" && cp "/Users/abbe/lab/notes/vault/projects/agent-setup/2026-03-30-agent-workspace-template-design.md" "/Users/abbe/Documents/agent-workspace/notes/vault/projects/agent-setup/" && cp "/Users/abbe/lab/notes/vault/projects/agent-setup/2026-03-30-agent-workspace-template-plan.md" "/Users/abbe/Documents/agent-workspace/notes/vault/projects/agent-setup/"
```

Expected: only the `agent-setup` documents are copied; no other vault note trees are brought over.

- [ ] **Step 5: Verify excluded real projects and private folders are still absent**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
root = Path('/Users/abbe/Documents/agent-workspace')
for rel in [
    'projects/finwin',
    'projects/teavis',
    '.claude',
    'notes/vault/sessions/teavis_first_steps.md',
]:
    path = root / rel
    print(rel, path.exists())
    assert not path.exists(), f'unexpected copied path: {path}'
PY
```

Expected: each listed path prints `False` and the script exits cleanly.

### Task 2: Add the public template files and safe ignore rules

**Files:**
- Create: `/Users/abbe/Documents/agent-workspace/README.md`
- Create: `/Users/abbe/Documents/agent-workspace/.gitignore`
- Create: `/Users/abbe/Documents/agent-workspace/notes/vault/sessions/.gitkeep`
- Create: `/Users/abbe/Documents/agent-workspace/notes/vault/logs/.gitkeep`
- Create: `/Users/abbe/Documents/agent-workspace/notes/vault/projects/.gitkeep`

- [ ] **Step 1: Write the template `.gitignore`**

Create `/Users/abbe/Documents/agent-workspace/.gitignore` with exactly:

```gitignore
.DS_Store
Thumbs.db
Desktop.ini

node_modules/
.next/
dist/
build/
.turbo/
coverage/

.claude/
.codex/
.opencode/

notes/vault/sessions/*
!notes/vault/sessions/.gitkeep
notes/vault/logs/*
!notes/vault/logs/.gitkeep
```

- [ ] **Step 2: Create the empty placeholder files for tracked structure-only directories**

Run:

```bash
mkdir -p "/Users/abbe/Documents/agent-workspace/notes/vault/sessions" && mkdir -p "/Users/abbe/Documents/agent-workspace/notes/vault/logs" && mkdir -p "/Users/abbe/Documents/agent-workspace/notes/vault/projects" && touch "/Users/abbe/Documents/agent-workspace/notes/vault/sessions/.gitkeep" "/Users/abbe/Documents/agent-workspace/notes/vault/logs/.gitkeep" "/Users/abbe/Documents/agent-workspace/notes/vault/projects/.gitkeep"
```

Expected: the directories exist with `.gitkeep` files only.

- [ ] **Step 3: Write the public `README.md`**

Create `/Users/abbe/Documents/agent-workspace/README.md` with exactly:

```md
# Agent Workspace

Agent Workspace is a reusable starter template for running software projects with Codex, OpenCode, and Claude-compatible instruction files.

It is built around a simple portable model:

- `AGENTS.md` is the canonical shared instruction file
- `.agents/skills/` holds portable workflow skills
- each project gets a `ledger.md` handoff file
- each project gets `docs/plan.md` and `docs/resources.md`
- notes live separately under `notes/vault/`

## Core idea

The goal is to keep active execution context close to the code while still giving agents a place for longer-term notes and planning.

The minimum recovery path is:

1. read `ledger.md`
2. read `docs/plan.md`
3. read `docs/resources.md`

Everything else is supplementary.

## Included in this template

- root `AGENTS.md`
- root `CLAUDE.md`
- portable skills in `.agents/skills/`
- `projects/example-project/` scaffold
- `notes/vault/projects/agent-setup/` example documentation

## Not included

- real project code
- private vault notes
- session dump content
- machine-local config folders

## Starting a new project

1. copy `projects/example-project/` to `projects/<your-project>/`
2. rename the project files and fill in the thesis, focus, and docs
3. keep `AGENTS.md` as the source of truth
4. add your own vault notes under `notes/vault/projects/` or your preferred note area

## Git behavior

This template ignores session dumps and local-only config so the repo can stay publishable.

## Agent compatibility

- Codex: reads `AGENTS.md`
- OpenCode: reads `AGENTS.md`
- Claude: can use `CLAUDE.md`, which should mirror `AGENTS.md`
```

- [ ] **Step 4: Verify the README and `.gitignore` contain the intended public-facing rules**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
checks = {
    '/Users/abbe/Documents/agent-workspace/README.md': [
        '`AGENTS.md` is the canonical shared instruction file',
        'projects/example-project/',
        'session dump content',
    ],
    '/Users/abbe/Documents/agent-workspace/.gitignore': [
        'notes/vault/sessions/*',
        '!notes/vault/sessions/.gitkeep',
        '.claude/',
    ],
}
for file_path, required in checks.items():
    text = Path(file_path).read_text()
    for needle in required:
        assert needle in text, f'missing {needle!r} in {file_path}'
        print('verified', file_path, needle)
PY
```

Expected: script exits cleanly and prints all `verified` lines.

### Task 3: Create the example project scaffold

**Files:**
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/AGENTS.md`
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/CLAUDE.md`
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/ledger.md`
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/README.md`
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/docs/plan.md`
- Create: `/Users/abbe/Documents/agent-workspace/projects/example-project/docs/resources.md`

- [ ] **Step 1: Write the example project `AGENTS.md`**

Create `/Users/abbe/Documents/agent-workspace/projects/example-project/AGENTS.md` with exactly:

```md
# Example Project

## Project Focus

This is a starter project scaffold for the Agent Workspace template.

## Working Rules

- Read `ledger.md` first when resuming work.
- Keep `docs/plan.md` and `docs/resources.md` current when the project changes.
- Treat `AGENTS.md` as the canonical shared instruction file.
- Keep `CLAUDE.md` aligned, but do not make it the source of truth.
```

- [ ] **Step 2: Write the example project `CLAUDE.md`**

Create `/Users/abbe/Documents/agent-workspace/projects/example-project/CLAUDE.md` with exactly:

```md
# Example Project

## Project Focus

This is a starter project scaffold for the Agent Workspace template.

## Working Rules

- Read `ledger.md` first when resuming work.
- Mirror the operating model in `AGENTS.md`.
- Keep `docs/plan.md` and `docs/resources.md` current when the project changes.
```

- [ ] **Step 3: Write the example project `ledger.md`**

Create `/Users/abbe/Documents/agent-workspace/projects/example-project/ledger.md` with exactly:

```md
# Example Project Ledger

## Thesis

- Replace this with the actual project thesis.

## Current Focus

- Replace this with the current focus area.

## 2026-03-30

- Initialized the example project scaffold.
- Next: replace this example content with your real project context.
```

- [ ] **Step 4: Write the example project `README.md`**

Create `/Users/abbe/Documents/agent-workspace/projects/example-project/README.md` with exactly:

```md
# Example Project

This directory is a starter scaffold for new projects created from the Agent Workspace template.

Start by filling in:
- `AGENTS.md`
- `CLAUDE.md`
- `ledger.md`
- `docs/plan.md`
- `docs/resources.md`
```

- [ ] **Step 5: Write `docs/plan.md` and `docs/resources.md` for the example project**

Create `/Users/abbe/Documents/agent-workspace/projects/example-project/docs/plan.md` with exactly:

```md
# Example Project Plan

## Thesis

Replace this with the actual project purpose.

## Current Direction

- Add the current priorities here.

## Near-Term Milestones

1. Replace with the first milestone.

## Success Signals

- Replace with practical outcomes that show the work is on track.
```
Create `/Users/abbe/Documents/agent-workspace/projects/example-project/docs/resources.md` with exactly:

```md
# Example Project Resources

## Key Files

- `ledger.md`
- `docs/plan.md`
- `docs/resources.md`

## Commands

- Add project commands here.

## Systems

- Add important services, links, and references here.
```

- [ ] **Step 6: Verify the example project demonstrates the ledger-first model**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
checks = {
    '/Users/abbe/Documents/agent-workspace/projects/example-project/AGENTS.md': ['Read `ledger.md` first when resuming work.'],
    '/Users/abbe/Documents/agent-workspace/projects/example-project/CLAUDE.md': ['Mirror the operating model in `AGENTS.md`.'],
    '/Users/abbe/Documents/agent-workspace/projects/example-project/ledger.md': ['Initialized the example project scaffold.'],
}
for file_path, required in checks.items():
    text = Path(file_path).read_text()
    for needle in required:
        assert needle in text, f'missing {needle!r} in {file_path}'
        print('verified', file_path, needle)
PY
```

Expected: script exits cleanly and prints all `verified` lines.

### Task 4: Initialize git and verify the tracked result is safe to publish

**Files:**
- Create: `/Users/abbe/Documents/agent-workspace/.git/`

- [ ] **Step 1: Initialize the new repo and inspect git status**

Run:

```bash
git init "/Users/abbe/Documents/agent-workspace" && git -C "/Users/abbe/Documents/agent-workspace" status --short
```

Expected: git initializes successfully and shows only the intended template files as untracked.

- [ ] **Step 2: Verify ignored content stays ignored and excluded content is absent**

Run:

```bash
git -C "/Users/abbe/Documents/agent-workspace" check-ignore -v notes/vault/sessions/test-session.md && python3 - <<'PY'
from pathlib import Path
root = Path('/Users/abbe/Documents/agent-workspace')
for rel in ['projects/finwin', 'projects/teavis', 'notes/vault/sessions/teavis_first_steps.md']:
    path = root / rel
    print(rel, path.exists())
    assert not path.exists(), f'unexpected private content present: {path}'
PY
```

Expected: `check-ignore` reports `.gitignore` is ignoring the test session path and the Python script confirms the excluded private paths are absent.

- [ ] **Step 3: Stage the template files and inspect the staged file list**

Run:

```bash
git -C "/Users/abbe/Documents/agent-workspace" add . && git -C "/Users/abbe/Documents/agent-workspace" diff --cached --name-only
```

Expected: the staged list contains only template files and does not include private project directories, session dump content, or machine-local config.

- [ ] **Step 4: Create the initial commit**

Run:

```bash
git -C "/Users/abbe/Documents/agent-workspace" commit -m "Create public agent workspace template"
```

Expected: the commit succeeds with the template files only.

- [ ] **Step 5: Verify the repo is clean after the commit**

Run:

```bash
git -C "/Users/abbe/Documents/agent-workspace" status --short
```

Expected: no output, indicating a clean working tree.

## Self-Review

- **Spec coverage:** This plan covers the clean export repo in `~/Documents/agent-workspace`, inclusion of root instructions and portable skills, retention of only `agent-setup` notes, exclusion of real projects and session content, addition of a public `README.md`, creation of a safe `.gitignore`, creation of `projects/example-project/`, and git initialization.
- **Placeholder scan:** Every step names exact files, exact commands, and exact file contents. No `TODO`, `TBD`, or implied follow-up placeholders remain.
- **Type consistency:** Paths, repo name, note folder names, and file roles are consistent throughout the plan.
