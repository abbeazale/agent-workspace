# Agent Workspace Template Design

## Goal

Create a clean public template repo in `~/Documents/agent-workspace` that captures the reusable cross-agent workspace structure without including private project data, personal vault content, or machine-local configuration.

The template should be suitable for publishing to GitHub and should help another developer adopt the same operating model with minimal cleanup.

## Decisions

### 1. Export target

- The template repo will live at `~/Documents/agent-workspace`.
- This will be a new repo directory, not a conversion of `/Users/abbe/lab`.
- The live `lab` workspace remains private and unmanaged by a top-level git repo.

### 2. Source of truth

- Keep `AGENTS.md` as the canonical instruction file.
- Keep `CLAUDE.md` as the compatibility mirror.
- Keep `.agents/skills/` as the portable workflow layer.
- Keep `ledger.md` and `docs/` as the project-level handoff and planning model.

### 3. What to include

The template repo should include:

- root `AGENTS.md`
- root `CLAUDE.md`
- `.agents/skills/`
- `notes/vault/projects/agent-setup/` as the example documentation area
- empty scaffolding for `notes/vault/sessions/`, `notes/vault/logs/`, and `notes/vault/projects/`
- a sample project scaffold such as `projects/example-project/`
- a public-facing `README.md`
- a `.gitignore` that excludes local-only and generated content

### 4. What to exclude

The template repo should not include:

- any real project repos such as `projects/finwin/` or `projects/teavis/`
- any personal vault content outside the `agent-setup` example docs
- session dump files inside `notes/vault/sessions/`
- machine-local config such as `.claude/`
- personal media files or unrelated workspace files
- local dependency caches or generated build outputs

### 5. Publishing posture

- This repo should read like a reusable starter kit, not a personal backup.
- Example docs should stay because they make the template concrete.
- Real user data and active project history should stay out.

## Approaches Considered

### Approach A: Clean template repo with example docs and example project

Create a new directory, copy only reusable structure, include one example project scaffold, and keep the `agent-setup` notes as public examples.

**Pros**
- Clean to publish
- Easy for others to understand
- Preserves the most useful working patterns

**Cons**
- Requires some copy and cleanup work

### Approach B: Minimal skeleton only

Ship just the top-level files, a few empty folders, and almost no example content.

**Pros**
- Very clean and lightweight

**Cons**
- Less helpful as a template
- Loses the practical examples that explain the operating model

### Approach C: Export from the live workspace with ignore rules

Initialize git in a directory shaped like the live workspace and use `.gitignore` to hide the private parts.

**Pros**
- Less immediate copying effort

**Cons**
- Too easy to leak private files
- Harder to reason about what is really included
- Confuses the template with the real workspace

### Recommendation

Use Approach A.

It gives the cleanest GitHub result while still keeping enough real structure and examples to be genuinely reusable.

## Proposed Template Structure

```text
agent-workspace/
|- AGENTS.md
|- CLAUDE.md
|- README.md
|- .gitignore
|- .agents/
|  |- skills/
|- projects/
|  |- example-project/
|     |- AGENTS.md
|     |- CLAUDE.md
|     |- ledger.md
|     |- README.md
|     |- docs/
|        |- plan.md
|        |- resources.md
|- notes/
   |- vault/
      |- sessions/
      |  |- .gitkeep
      |- logs/
      |  |- .gitkeep
      |- projects/
         |- .gitkeep
         |- agent-setup/
            |- 2026-03-29-phase-2-memory-setup-design.md
            |- 2026-03-29-phase-2-memory-setup-plan.md
            |- 2026-03-30-equilibrium-work-laptop-setup.md
```

## README Direction

The README should explain:

- what this template is
- the core operating model
- how `AGENTS.md`, `.agents/skills/`, `ledger.md`, and `docs/` work together
- how to create a new project from `projects/example-project/`
- what is intentionally ignored by git
- how to adapt it for Codex, OpenCode, and Claude

It should be written for someone discovering the template fresh on GitHub.

## `.gitignore` Direction

The template `.gitignore` should at minimum ignore:

- `notes/vault/sessions/*`
- `!notes/vault/sessions/.gitkeep`
- `.DS_Store`
- `node_modules/`
- `.next/`
- build outputs and temp files
- local agent config folders that should not be shared

Because real projects will live under `projects/`, the ignore file should avoid making assumptions that would block normal project repos from being tracked later.

## Example Project Strategy

- Replace real projects with `projects/example-project/`.
- The example project should demonstrate the required files only.
- It should not contain app code.
- It should explain the ledger-first recovery path.

## Risks and Guardrails

- Do not accidentally copy private vault notes outside `agent-setup`.
- Do not include real project repos or nested `.git/` directories.
- Do not include session dump content.
- Do not include machine-specific config from `.claude/`.
- Keep the template small enough to understand quickly.

## Success Criteria

- `~/Documents/agent-workspace` contains only reusable public template material.
- The repo can be initialized safely without leaking private notes or project code.
- The README explains the model clearly enough for a new user to adopt it.
- `notes/vault/sessions/` exists in the repo as structure only, with no session content tracked.
