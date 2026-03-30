# Phase 2 Memory Setup Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Strengthen the portable `AGENTS.md`-first workflow for Codex and OpenCode while adding thin Claude session-dump memory that writes into `notes/vault/sessions/`.

**Architecture:** Keep `AGENTS.md` as the canonical shared instruction file and treat `CLAUDE.md` as a mirrored compatibility entrypoint. Add Codex-global guidance in `~/.codex/instructions.md`, keep OpenCode on the existing `AGENTS.md` path, and wire Claude memory through `SESSIONS_DIR`, QMD indexing, and the `agent-memory` plugin without making Claude-only features required for normal project recovery.

**Tech Stack:** Markdown docs, Claude Code plugin install, zsh shell config, Codex global instructions, QMD CLI

---

## File Map

- Modify: `/Users/abbe/lab/AGENTS.md` - make the canonical instruction priority and tool-choice rules explicit.
- Modify: `/Users/abbe/lab/CLAUDE.md` - mirror the shared operating model and Claude recovery workflow without becoming the source of truth.
- Modify: `/Users/abbe/.zshrc` - persist `SESSIONS_DIR` for Claude memory dumps.
- Create: `/Users/abbe/.codex/instructions.md` - lightweight Codex-global note that points back to `AGENTS.md` and the ledger-first workflow.
- Modify: `/Users/abbe/lab/notes/vault/codex-setup.md` - replace the empty stub with the primary Codex/OpenCode operating notes for this workspace.
- Verify against: `/Users/abbe/lab/notes/vault/projects/agent-setup/2026-03-29-phase-2-memory-setup-design.md`

### Task 1: Tighten the shared workspace instructions

**Files:**
- Modify: `/Users/abbe/lab/AGENTS.md`
- Modify: `/Users/abbe/lab/CLAUDE.md`

- [ ] **Step 1: Verify the current instruction files do not already state the canonical-source rule and tool-selection rule clearly**

Run:

```bash
python3 - <<'PY'
from pathlib import Path

for path in [Path('/Users/abbe/lab/AGENTS.md'), Path('/Users/abbe/lab/CLAUDE.md')]:
    text = path.read_text()
    print(path.name)
    print('has canonical-source wording:', 'canonical' in text.lower() and 'AGENTS.md' in text)
    print('has tool-selection wording:', 'use `Read`' in text or 'use `Read` to inspect the file' in text)
    print('---')
PY
```

Expected: both files print `has canonical-source wording: False` or otherwise show the wording is incomplete enough to justify the edit.

- [ ] **Step 2: Update `/Users/abbe/lab/AGENTS.md` with the canonical-source and tool-selection sections**

Insert the following sections into the existing document in the same style as the rest of the file:

```md
### Instruction Priority

- `AGENTS.md` is the canonical shared instruction file for this workspace.
- `CLAUDE.md` exists for Claude Code compatibility and should mirror the same operating model.
- When shared workflow rules change, update `AGENTS.md` first and keep `CLAUDE.md` aligned.

### Tool Selection

- If an exact file path is known, use `Read`.
- Use `Glob` to find files by name or path pattern.
- Use `Grep` to find text across files.
- Use `Bash` for terminal execution like git, builds, package scripts, and agent CLIs, not routine file reading.
```

- [ ] **Step 3: Update `/Users/abbe/lab/CLAUDE.md` so it mirrors the same rules without becoming a second source of truth**

Insert the following sections into the existing document in the same style as the rest of the file:

```md
### Instruction Priority

- `AGENTS.md` is the canonical shared instruction file for this workspace.
- `CLAUDE.md` mirrors the same operating model for Claude Code compatibility.
- When shared workflow rules change, update `AGENTS.md` first and keep this file aligned.

### Tool Selection

- If an exact file path is known, use `Read`.
- Use `Glob` to find files by name or path pattern.
- Use `Grep` to find text across files.
- Use `Bash` for terminal execution like git, builds, package scripts, and agent CLIs, not routine file reading.
```

- [ ] **Step 4: Verify both files contain the new rules exactly once**

Run:

```bash
python3 - <<'PY'
from pathlib import Path

checks = {
    '/Users/abbe/lab/AGENTS.md': [
        '`AGENTS.md` is the canonical shared instruction file for this workspace.',
        '- If an exact file path is known, use `Read`.',
    ],
    '/Users/abbe/lab/CLAUDE.md': [
        '`AGENTS.md` is the canonical shared instruction file for this workspace.',
        '- If an exact file path is known, use `Read`.',
    ],
}

for file_path, required in checks.items():
    text = Path(file_path).read_text()
    for needle in required:
        count = text.count(needle)
        print(file_path, repr(needle), count)
        assert count == 1, f'{file_path} expected exactly one copy of {needle!r}, found {count}'
PY
```

Expected: script exits cleanly with each required string reported exactly once.

### Task 2: Add Codex-first global guidance that points back to the portable layer

**Files:**
- Create: `/Users/abbe/.codex/instructions.md`
- Modify: `/Users/abbe/lab/notes/vault/codex-setup.md`

- [ ] **Step 1: Confirm the current Codex global instructions file is empty and the vault note is unused**

Run:

```bash
python3 - <<'PY'
from pathlib import Path

for path in [
    Path('/Users/abbe/.codex/instructions.md'),
    Path('/Users/abbe/lab/notes/vault/codex-setup.md'),
]:
    text = path.read_text() if path.exists() else ''
    print(path, 'exists=', path.exists(), 'chars=', len(text))
PY
```

Expected: `~/.codex/instructions.md` exists with `chars= 0` and `codex-setup.md` is empty or nearly empty.

- [ ] **Step 2: Write `/Users/abbe/.codex/instructions.md` with the Codex-global rules for this workspace**

Write exactly:

```md
# Codex Workspace Notes

- When working in `/Users/abbe/lab`, treat `/Users/abbe/lab/AGENTS.md` as the canonical shared instruction file.
- Read the relevant project `ledger.md` first, then `docs/plan.md`, then `docs/resources.md`.
- If an exact file path is known, use `Read` instead of `Bash`.
- Use `Glob` to find files by path pattern.
- Use `Grep` to find text across files.
- Use `Bash` for git, builds, scripts, package managers, and other terminal operations.
- Treat Claude-only memory as optional. The portable truth is `AGENTS.md`, project docs, and `ledger.md`.
```

- [ ] **Step 3: Replace `/Users/abbe/lab/notes/vault/codex-setup.md` with the day-to-day Codex/OpenCode workflow note**

Write exactly:

```md
# Codex and OpenCode Setup

## Source of truth

- `AGENTS.md` is the canonical workspace instruction file.
- Project `AGENTS.md` files hold project-specific shared rules.
- `ledger.md` is the first project handoff file to read.

## Default recovery path

1. Read the relevant project `ledger.md`.
2. Read `docs/plan.md`.
3. Read `docs/resources.md`.
4. Read recent `notes/vault/sessions/` files only when extra session context is actually needed.

## Tool preference

- Use `Read` when you already know the path.
- Use `Glob` to find files.
- Use `Grep` to search file contents.
- Use `Bash` for execution, not routine file inspection.

## Notes

- OpenCode reads `AGENTS.md` directly, so it does not need a separate instruction template in this setup.
- Claude memory is a supplement, not the primary recovery path.
```

- [ ] **Step 4: Verify both Codex-facing files contain the expected guidance**

Run:

```bash
python3 - <<'PY'
from pathlib import Path

checks = {
    '/Users/abbe/.codex/instructions.md': [
        'treat `/Users/abbe/lab/AGENTS.md` as the canonical shared instruction file.',
        'If an exact file path is known, use `Read` instead of `Bash`.',
        'The portable truth is `AGENTS.md`, project docs, and `ledger.md`.',
    ],
    '/Users/abbe/lab/notes/vault/codex-setup.md': [
        '`AGENTS.md` is the canonical workspace instruction file.',
        'OpenCode reads `AGENTS.md` directly, so it does not need a separate instruction template in this setup.',
    ],
}

for file_path, required in checks.items():
    text = Path(file_path).read_text()
    for needle in required:
        assert needle in text, f'missing {needle!r} in {file_path}'
        print('verified', file_path, 'contains', needle)
PY
```

Expected: script exits cleanly and prints all `verified` lines.

### Task 3: Wire Claude session dumps into the vault without making them required

**Files:**
- Modify: `/Users/abbe/.zshrc`

- [ ] **Step 1: Verify the session dump environment variable is not already set in `/Users/abbe/.zshrc`**

Run:

```bash
python3 - <<'PY'
from pathlib import Path

text = Path('/Users/abbe/.zshrc').read_text()
needle = 'export SESSIONS_DIR="$HOME/lab/notes/vault/sessions"'
print('already present:', needle in text)
assert needle not in text, 'SESSIONS_DIR export already exists; stop and update the plan before duplicating it'
PY
```

Expected: `already present: False`.

- [ ] **Step 2: Append the session-dump export to `/Users/abbe/.zshrc`**

Append exactly:

```bash

# Claude session dumps for workspace memory
export SESSIONS_DIR="$HOME/lab/notes/vault/sessions"
```

- [ ] **Step 3: Create and verify the target sessions directory**

Run:

```bash
mkdir -p "/Users/abbe/lab/notes/vault/sessions" && ls -ld "/Users/abbe/lab/notes/vault/sessions"
```

Expected: `ls -ld` prints the directory metadata for `/Users/abbe/lab/notes/vault/sessions`.

- [ ] **Step 4: Reload the shell config and verify the environment variable resolves correctly**

Run:

```bash
zsh -lc 'source ~/.zshrc && printf "%s\n" "$SESSIONS_DIR"'
```

Expected: `/Users/abbe/lab/notes/vault/sessions`

### Task 4: Install and verify the minimal Claude memory tooling

**Files:**
- Modify: `/Users/abbe/lab/AGENTS.md`
- Modify: `/Users/abbe/lab/CLAUDE.md`
- Modify: `/Users/abbe/.zshrc`

- [ ] **Step 1: Install QMD globally for vault indexing**

Run:

```bash
npm install -g @tobilu/qmd
```

Expected: npm reports a successful global install for `@tobilu/qmd`.

- [ ] **Step 2: Verify QMD is available on the shell path**

Run:

```bash
zsh -lc 'source ~/.zshrc && command -v qmd'
```

Expected: a path ending in `/qmd` is printed.

- [ ] **Step 3: Index the vault so session dumps become searchable**

Run:

```bash
qmd index "/Users/abbe/lab/notes/vault"
```

Expected: QMD completes without error and reports that the vault was indexed or updated.

- [ ] **Step 4: Install the Claude memory plugin from the agent-utils marketplace**

Run:

```bash
claude plugin marketplace add k3nnethfrancis/agent-utils && claude plugin install agent-memory@agent-utils
```

Expected: Claude reports a successful marketplace add and a successful `agent-memory` plugin install.

- [ ] **Step 5: Verify Claude sees the installed plugin**

Run:

```bash
claude plugin list
```

Expected: the output includes `agent-memory`.

- [ ] **Step 6: Update the shared docs so Claude memory is clearly described as secondary to the ledger**

Add the following bullet under the session-memory guidance in both `/Users/abbe/lab/AGENTS.md` and `/Users/abbe/lab/CLAUDE.md`:

```md
- For normal project recovery, read the project `ledger.md` first; use `notes/vault/sessions/` as supplemental session context.
```

- [ ] **Step 7: Verify the docs and environment now match the intended recovery model**

Run:

```bash
python3 - <<'PY'
from pathlib import Path

required = '- For normal project recovery, read the project `ledger.md` first; use `notes/vault/sessions/` as supplemental session context.'
for file_path in ['/Users/abbe/lab/AGENTS.md', '/Users/abbe/lab/CLAUDE.md']:
    text = Path(file_path).read_text()
    assert required in text, f'missing ledger-first recovery note in {file_path}'

zshrc = Path('/Users/abbe/.zshrc').read_text()
assert 'export SESSIONS_DIR="$HOME/lab/notes/vault/sessions"' in zshrc
print('ledger-first docs and SESSIONS_DIR verified')
PY
```

Expected: script exits cleanly and prints `ledger-first docs and SESSIONS_DIR verified`.

### Task 5: Final verification of the portable-first workflow

**Files:**
- Verify: `/Users/abbe/lab/AGENTS.md`
- Verify: `/Users/abbe/lab/CLAUDE.md`
- Verify: `/Users/abbe/.codex/instructions.md`
- Verify: `/Users/abbe/lab/notes/vault/codex-setup.md`
- Verify: `/Users/abbe/.zshrc`

- [ ] **Step 1: Run a single verification script across all modified files**

Run:

```bash
python3 - <<'PY'
from pathlib import Path

checks = {
    '/Users/abbe/lab/AGENTS.md': [
        '`AGENTS.md` is the canonical shared instruction file for this workspace.',
        '- If an exact file path is known, use `Read`.',
        'use `notes/vault/sessions/` as supplemental session context.',
    ],
    '/Users/abbe/lab/CLAUDE.md': [
        '`AGENTS.md` is the canonical shared instruction file for this workspace.',
        '- If an exact file path is known, use `Read`.',
        'use `notes/vault/sessions/` as supplemental session context.',
    ],
    '/Users/abbe/.codex/instructions.md': [
        'treat `/Users/abbe/lab/AGENTS.md` as the canonical shared instruction file.',
        'If an exact file path is known, use `Read` instead of `Bash`.',
    ],
    '/Users/abbe/lab/notes/vault/codex-setup.md': [
        '`AGENTS.md` is the canonical workspace instruction file.',
        'Claude memory is a supplement, not the primary recovery path.',
    ],
    '/Users/abbe/.zshrc': [
        'export SESSIONS_DIR="$HOME/lab/notes/vault/sessions"',
    ],
}

for file_path, required in checks.items():
    text = Path(file_path).read_text()
    for needle in required:
        assert needle in text, f'missing {needle!r} in {file_path}'
        print('ok', file_path, needle)
PY
```

Expected: script exits cleanly and prints `ok` for every required string.

- [ ] **Step 2: Verify the Claude memory command path is ready**

Run:

```bash
zsh -lc 'source ~/.zshrc && printf "SESSIONS_DIR=%s\n" "$SESSIONS_DIR" && command -v qmd'
```

Expected: the first line prints `SESSIONS_DIR=/Users/abbe/lab/notes/vault/sessions` and the second line prints the path to `qmd`.

- [ ] **Step 3: Record the completion note in the design area**

Append the following section to `/Users/abbe/lab/notes/vault/projects/agent-setup/2026-03-29-phase-2-memory-setup-design.md`:

```md
## Implementation Status

- Phase 2 portable-first setup completed.
- `AGENTS.md` remains canonical.
- Codex and OpenCode continue using the portable workflow.
- Claude session dumps write into `notes/vault/sessions/` as supplemental memory.
```

- [ ] **Step 4: Verify the status note was appended exactly once**

Run:

```bash
python3 - <<'PY'
from pathlib import Path

path = Path('/Users/abbe/lab/notes/vault/projects/agent-setup/2026-03-29-phase-2-memory-setup-design.md')
needle = '## Implementation Status'
text = path.read_text()
count = text.count(needle)
print('Implementation Status count:', count)
assert count == 1, f'expected exactly one Implementation Status section, found {count}'
PY
```

Expected: `Implementation Status count: 1`

## Self-Review

- **Spec coverage:** The plan covers the canonical-instruction decision, the Codex/OpenCode-first workflow, the thin Claude memory addition, the `Read`/`Glob`/`Grep` guidance, and the requirement that Claude-only memory never replace `ledger.md`.
- **Placeholder scan:** No `TBD`, `TODO`, or deferred implementation placeholders remain; each task names exact files, commands, and text.
- **Type consistency:** File paths, environment variables, and canonical wording stay consistent across all tasks.
