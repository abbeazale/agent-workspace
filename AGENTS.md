# Workspace

This is the root workspace. It keeps project repos and long-term notes side by side so Claude Code, Codex, and OpenCode can all share the same operating model.

---

## Structure

```
lab/
├── AGENTS.md               # workspace instructions for Codex/OpenCode
├── CLAUDE.md               # workspace instructions for Claude Code
├── .agents/
│   └── skills/             # portable skills shared across agents
├── projects/               # code lives here, each project has its own repo
│   └── {project-name}/
│       ├── AGENTS.md       # project-specific instructions for Codex/OpenCode
│       ├── CLAUDE.md       # project-specific instructions for Claude Code
│       ├── ledger.md       # universal session handoff
│       ├── README.md
│       └── docs/
│           ├── plan.md
│           └── resources.md
└── notes/
    └── vault/             # markdown knowledge layer
        ├── sessions/      # session dumps
        ├── logs/          # weekly/monthly summaries
        └── projects/      # planning docs, specs, research notes
```

### Why This Layout

Code and notes live in separate trees but stay connected through shared markdown files. The portable layer is `AGENTS.md` + `.agents/skills/` + `ledger.md`; everything else is optional enhancement.

**`lab/` is NOT a git repo.** Each project repo and the vault manage their own history.

### Instruction Priority

- `AGENTS.md` is the canonical shared instruction file for this workspace.
- `CLAUDE.md` exists for Claude Code compatibility and should mirror the same operating model.
- When shared workflow rules change, update `AGENTS.md` first and keep `CLAUDE.md` aligned.

### Tool Selection

- If an exact file path is known, use `Read`.
- Use `Glob` to find files by name or path pattern.
- Use `Grep` to find text across files.
- Use `Bash` for terminal execution like git, builds, package scripts, and agent CLIs, not routine file reading.

---

## Core Files

### Project Ledgers

Each project keeps a `ledger.md` at its root. This is the universal memory layer across agents and sessions.

**Read it first when entering a project. Update it before ending a session or compacting context.**

Append new entries at the top under a dated heading. Do not rewrite older history unless asked.

### Portable Skills

Portable skills live in `.agents/skills/` and are the shared workflow layer across Claude Code, Codex, and OpenCode.

Current custom workflow skills:
- `session-handoff` for end-of-session ledger and docs updates
- `update-docs` for milestone and feature documentation updates

### Vault Logs

`notes/vault/logs/` contains:
- archived daily notes
- weekly summaries
- monthly summaries

---

## Working Protocols

### Finding Project Context

When asked to work on a project:
1. Read `projects/{project-name}/ledger.md` first.
2. Read `projects/{project-name}/docs/plan.md`.
3. Read `projects/{project-name}/docs/resources.md`.
4. Check `notes/vault/projects/{project-name}/` for specs or research when it exists.

### Documentation Workflow

- When a feature or meaningful milestone lands, invoke `update-docs`.
- Before ending work or compacting context, invoke `session-handoff`.
- If priorities or architecture shift, update `docs/plan.md` and `docs/resources.md` in the project repo.

### Session Memory

Conversation history can be dumped to `notes/vault/sessions/{session_id}.md` before compaction. This is the recovery layer for long sessions and agent switches.

- For normal project recovery, read the project `ledger.md` first; use `notes/vault/sessions/` as supplemental session context.

### Memory Architecture

Four layers of persistent memory, each serving a different purpose:

| Layer | Location | Purpose | Loaded |
|---|---|---|---|
| **Ledger** | `projects/{name}/ledger.md` | Cross-agent handoff and current focus | On demand |
| **Agent memory** | Agent-specific memory location (e.g. `~/.claude/...`, `~/.codex/...`, or project-local) | Stable agent-specific facts | Agent-specific |
| **Session dumps** | `notes/vault/sessions/{id}.md` | Full conversation recovery | On demand |
| **Vault** | `notes/vault/` | Long-term knowledge, planning, research | On demand |

The ledger is the required handoff layer. Everything else is supplemental.

### Note-Taking

Default to writing loose markdown notes in the vault, then organize later in batches. Search is more important than rigid upfront taxonomy.

### Project Kickoff

When starting a new project:
1. Create `projects/{project-name}/` with `AGENTS.md`, `CLAUDE.md`, `ledger.md`, `README.md`, `docs/plan.md`, and `docs/resources.md`.
2. Initialize `ledger.md` with the project thesis, current focus, and a dated setup entry.
3. Optionally create `notes/vault/projects/{project-name}/` for supporting notes and specs.

---

## Setup

1. Create the workspace structure:
   ```bash
   mkdir -p workspace/{projects,notes/vault/{sessions,logs,projects},automations}
   cp this-file workspace/AGENTS.md
   cp this-file workspace/CLAUDE.md
   ```

2. Initialize the vault as a git repo:
   ```bash
   cd workspace/notes/vault
   git init
   ```

3. Install memory hooks later if needed:
   ```bash
   # Claude-only example
   claude plugin add path/to/agent-utils/plugins/agent-memory
   export SESSIONS_DIR=~/path/to/workspace/notes/vault/sessions
   ```

4. Optionally index the vault for search:
   ```bash
   bun install -g @tobilu/qmd
   qmd index workspace/notes/vault
   ```

Obsidian is a good fit for the vault, but any git-tracked markdown directory works.
