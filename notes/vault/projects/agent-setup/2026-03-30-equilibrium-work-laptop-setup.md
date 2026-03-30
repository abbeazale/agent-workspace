# Equilibrium Work Laptop Setup

This guide adapts the current cross-agent setup to your work laptop.

It assumes:
- Windows machine
- PowerShell-first workflow
- one project repo
- one separate Obsidian vault
- the project note area inside the vault is `equilibriummining/`
- Codex and OpenCode are the primary tools
- Claude compatibility is nice to have, not the center of the setup

## Goal

Create a simple, durable setup where:
- the project repo holds the operational truth for active work
- the Obsidian vault holds long-term notes and research
- Codex and OpenCode can resume cleanly from the same files
- git initialization happens last, after the structure is right

## Recommended Structure

### Project repo

Use one repo root for the Equilibrium project, for example:

```text
C:\Users\<you>\work\equilibrium\
```

Inside the repo:

```text
equilibrium/
|- AGENTS.md
|- CLAUDE.md
|- ledger.md
|- README.md
|- docs/
|  |- plan.md
|  |- resources.md
```

### Obsidian vault

Keep the vault separate from the repo, for example:

```text
C:\Users\<you>\Documents\ObsidianVault\
```

Inside that vault, keep the project area here:

```text
ObsidianVault/
|- equilibriummining/
|  |- overview.md
|  |- specs/
|  |- research/
|  |- decisions/
```

You do not need a `projects/` folder in the vault for this setup. The project note home is just `equilibriummining/`.

## What Lives Where

### In the repo

Use the repo for active execution context:
- `AGENTS.md` - canonical shared instructions for Codex and OpenCode
- `CLAUDE.md` - Claude-compatible mirror of the same operating model
- `ledger.md` - universal handoff file and first thing to read when resuming work
- `docs/plan.md` - current direction, milestones, priorities
- `docs/resources.md` - key commands, systems, references, links, environment notes

### In the vault

Use `equilibriummining/` for longer-lived knowledge:
- product notes
- research
- architecture thinking
- meeting notes
- decision logs
- specs you want to keep outside the repo

## Recovery Order for Codex and OpenCode

When resuming work on the laptop, the default path should be:

1. Read `ledger.md`
2. Read `docs/plan.md`
3. Read `docs/resources.md`
4. Read vault notes in `equilibriummining/` only when extra long-term context is needed

That keeps the portable workflow fast and avoids making the vault the first place the agent has to dig.

## File Contents to Start With

### `AGENTS.md`

Use this as the canonical file.

Suggested starting content:

```md
# Equilibrium

## Project Focus

Equilibrium is the primary work project on this machine.

## Working Rules

- Read `ledger.md` first when resuming work.
- Keep `docs/plan.md` and `docs/resources.md` current when priorities, architecture, or setup changes.
- Treat `AGENTS.md` as the canonical shared instruction file.
- Keep `CLAUDE.md` aligned, but do not make it the source of truth.
- If an exact file path is known, use `Read`.
- Use `Glob` to find files by path.
- Use `Grep` to search file contents.
- Use `Bash` only for actual terminal execution such as git, builds, scripts, and package managers.
```

### `CLAUDE.md`

Keep this very close to `AGENTS.md`.

Suggested starting content:

```md
# Equilibrium

## Project Focus

Equilibrium is the primary work project on this machine.

## Working Rules

- Read `ledger.md` first when resuming work.
- Mirror the operating model in `AGENTS.md`.
- Keep `docs/plan.md` and `docs/resources.md` current when priorities, architecture, or setup changes.
- If an exact file path is known, use `Read`.
- Use `Glob` to find files by path.
- Use `Grep` to search file contents.
- Use `Bash` only for actual terminal execution such as git, builds, scripts, and package managers.
```

### `ledger.md`

Suggested starting content:

```md
# Equilibrium Ledger

## Thesis

- Define the core purpose of the project in one or two bullets.

## Current Focus

- List the current active area of work.

## 2026-03-30

- Initialized the repo-level operating files for cross-agent work on the Windows laptop.
- Connected the repo workflow to the separate Obsidian vault folder `equilibriummining`.
- Next: fill in the real project thesis, current priorities, and the first active milestone.
```

### `docs/plan.md`

Suggested starting content:

```md
# Equilibrium Plan

## Thesis

Write the short statement of what this project is supposed to do.

## Current Direction

- What matters now
- What is intentionally deferred

## Near-Term Milestones

1. First real milestone
2. Second real milestone

## Success Signals

- What would make this setup feel useful in practice
```

### `docs/resources.md`

Suggested starting content:

```md
# Equilibrium Resources

## Key Files

- `ledger.md`
- `docs/plan.md`
- `docs/resources.md`

## Commands

- add your main dev commands here

## Systems

- list important services, APIs, repos, dashboards, or internal references

## Notes

- add setup details that future sessions should not have to rediscover
```

## PowerShell Setup Steps

Run these after choosing your actual repo path.

### 1. Create the repo folder structure

```powershell
$Repo = "C:\Users\<you>\work\equilibrium"
New-Item -ItemType Directory -Force -Path $Repo | Out-Null
New-Item -ItemType Directory -Force -Path "$Repo\docs" | Out-Null
```

### 2. Create the initial files

```powershell
New-Item -ItemType File -Force -Path "$Repo\AGENTS.md" | Out-Null
New-Item -ItemType File -Force -Path "$Repo\CLAUDE.md" | Out-Null
New-Item -ItemType File -Force -Path "$Repo\ledger.md" | Out-Null
New-Item -ItemType File -Force -Path "$Repo\README.md" | Out-Null
New-Item -ItemType File -Force -Path "$Repo\docs\plan.md" | Out-Null
New-Item -ItemType File -Force -Path "$Repo\docs\resources.md" | Out-Null
```

### 3. Make the Obsidian project area if it does not exist yet

```powershell
$Vault = "C:\Users\<you>\Documents\ObsidianVault"
New-Item -ItemType Directory -Force -Path "$Vault\equilibriummining" | Out-Null
New-Item -ItemType Directory -Force -Path "$Vault\equilibriummining\specs" | Out-Null
New-Item -ItemType Directory -Force -Path "$Vault\equilibriummining\research" | Out-Null
New-Item -ItemType Directory -Force -Path "$Vault\equilibriummining\decisions" | Out-Null
```

After that, copy or move your existing Equilibrium notes into `equilibriummining/`.

## Codex Setup on Windows

Codex should stay pointed at the repo-level `AGENTS.md`.

Suggested global instructions file:

```text
C:\Users\<you>\.codex\instructions.md
```

Suggested content:

```md
# Codex Workspace Notes

- When working in the Equilibrium repo, treat `AGENTS.md` as the canonical instruction file.
- Read `ledger.md` first, then `docs/plan.md`, then `docs/resources.md`.
- Use vault notes in `equilibriummining/` only when longer-term context is needed.
- If an exact file path is known, use `Read` instead of shell-based file inspection.
```

## OpenCode Setup on Windows

OpenCode should also rely on the repo-level `AGENTS.md`.

Suggested global config location:

```text
C:\Users\<you>\.config\opencode\opencode.json
```

You do not need a separate OpenCode-specific instruction template if `AGENTS.md` is present in the repo.

## Optional WSL Note

If you later move agent work into WSL:
- keep the same repo file structure
- keep `AGENTS.md` canonical
- keep the Obsidian vault separate
- adjust paths only

Do not redesign the whole system just because the shell changes.

## Git Comes Last

Only do this after the structure and docs are in place.

From PowerShell:

```powershell
Set-Location "C:\Users\<you>\work\equilibrium"
git init
git add .
git commit -m "Initialize cross-agent project setup"
```

If this repo will sync to another machine later:
- create the remote after the local structure feels right
- push only after you are happy with the repo contents
- make sure you are not accidentally committing personal-only files

## Recommended Order of Work

1. Create the repo structure
2. Create the core files
3. Import or organize the Obsidian notes into `equilibriummining/`
4. Fill in the real `ledger.md`, `docs/plan.md`, and `docs/resources.md`
5. Add Codex global instructions
6. Confirm OpenCode reads `AGENTS.md`
7. Initialize git last

## Questions to Answer Later

These can stay open for now, but should eventually be filled in:
- What is the exact Windows path for the project repo?
- What is the exact Windows path for the Obsidian vault?
- Are there any internal company docs, dashboards, or links that belong in `docs/resources.md`?
- Do you want meeting notes and research to stay mixed together in `equilibriummining/`, or separated into subfolders?
- Will you ever want session dumps or agent-specific memory on this machine, or should the workflow stay fully portable?
- Are there any work-machine restrictions around git remotes, package installs, or local tooling?

## The Core Principle

Keep the setup boring and obvious.

The portable truth should always be:
- `AGENTS.md`
- `ledger.md`
- `docs/plan.md`
- `docs/resources.md`

Everything else should support that, not compete with it.
