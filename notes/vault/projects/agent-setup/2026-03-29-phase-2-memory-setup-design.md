# Phase 2 Memory Setup Design

## Goal

Add the next layer of cross-agent setup without breaking the simple portable foundation from Phase 1.

This phase should keep `AGENTS.md` as the shared instruction source of truth, prioritize the workflows that matter most for Codex and OpenCode, and avoid adding unnecessary OpenCode-specific files.

## Decisions

### 1. Instruction source of truth

- `AGENTS.md` remains the canonical cross-agent instruction file.
- `CLAUDE.md` remains present for Claude Code compatibility, but mirrors the same operating model instead of introducing a competing workflow.
- OpenCode needs no separate instruction template because it already reads `AGENTS.md`.
- Codex should use minimal project or workspace config only where agent-specific behavior is actually needed.

### 2. Memory model

- `ledger.md` remains the universal handoff layer across all agents and sessions.
- Claude-side session dumps are an enhancement for compaction recovery, not a replacement for the ledger.
- Session dumps should live in `notes/vault/sessions/` so they stay in the existing knowledge layer.
- Agent-specific memory should be treated as supplementary and never as the only place where important project state lives.

### 3. Primary operator path

- The expected day-to-day workflow is mainly Codex and OpenCode, not Claude Code.
- That makes the portable layer more important than any Claude-specific memory feature.
- `AGENTS.md`, `.agents/skills/`, project docs, and `ledger.md` should stay sufficient for normal use even if Claude memory is never consulted.
- Claude memory work is still useful, but it should stay thin and clearly secondary.

### 4. Scope boundaries

This phase includes:

- Claude memory and session-dump setup
- small instruction updates where the workflow needs to be made more explicit
- lightweight Codex scaffolding only if needed to align behavior with `AGENTS.md`
- explicit tool guidance on when to use `Read`, `Glob`, `Grep`, and when not to use `Bash`

This phase does not include:

- backfilling the rest of the project ledgers or docs
- a large Codex rules system
- an OpenCode plugin or separate OpenCode instruction layer
- semantic vault indexing or other Phase 4 maintenance work

## Approaches Considered

### Approach A: Portable-first setup, keep `AGENTS.md` canonical, add thin Claude memory

Keep `AGENTS.md` as the portable truth, make sure Codex and OpenCode workflows stay clean, and add Claude-side session recovery only as a thin supplement.

**Pros**
- Matches how Codex and OpenCode already work
- Improves daily usability quickly
- Avoids unnecessary duplication and agent-specific drift
- Fits the fact that Codex and OpenCode are the primary tools

**Cons**
- Claude memory still adds agent-specific setup that Codex and OpenCode do not share

### Approach B: Make `CLAUDE.md` canonical and point others to it

Put the real instructions in `CLAUDE.md` and have `AGENTS.md` act as a wrapper.

**Pros**
- Slightly easier to think about from the Claude side

**Cons**
- Worse fit for Codex
- Adds indirection where none is needed
- Makes the portable file less authoritative

### Approach C: Build per-agent config layers now

Create fuller Codex and OpenCode infrastructure alongside Claude memory setup.

**Pros**
- More complete agent-specific preparation up front

**Cons**
- Higher maintenance overhead
- More moving pieces before the workflow is proven
- Violates the original recommendation to avoid elaborate hook systems early

### Recommendation

Use Approach A.

It preserves the best property of the current setup: one portable source of truth plus one universal handoff file. That matters even more when Codex and OpenCode are the primary tools. Claude can still gain better session recovery, but only as a supplement to the portable workflow.

## Proposed Architecture

### Shared layer

- `AGENTS.md` defines the portable workspace rules.
- project `AGENTS.md` files define project-specific portable rules.
- project `ledger.md` files remain the cross-agent handoff entry point.
- `.agents/skills/` remains the portable workflow layer.

### Claude layer

- `CLAUDE.md` mirrors the shared operating model.
- Claude config should point session dumps to `notes/vault/sessions/`.
- If hooks or plugin wiring are needed, they should be minimal and only support session recovery and reinjection.
- Claude-specific memory should not become required for normal project resumption.

### Codex layer

- Only add minimal scaffolding where it clarifies behavior or reduces friction.
- Any Codex-specific config should reference the shared conventions rather than restating project rules.
- Because Codex is a primary tool, its path to `AGENTS.md`, `ledger.md`, and project docs should stay especially obvious.

### OpenCode layer

- No dedicated template is needed in this phase.
- OpenCode should continue consuming `AGENTS.md` directly.

## Tool Guidance

When an exact file path is known:

- use `Read` to inspect the file
- do not use `Bash` to `cat` or search for it

When the filename is known but not the exact path:

- use `Glob`

When the content is known but the file is not:

- use `Grep`

When terminal execution is actually needed:

- use `Bash` for commands like git, package scripts, builds, or agent CLIs

This should be captured in the instructions because it reduces noise, avoids unnecessary shell work, and is usually the cheaper path when the target file is already known.

## Implementation Plan for Phase 2

1. Update workspace docs so `AGENTS.md` is explicitly described as canonical and `CLAUDE.md` as the mirrored Claude entrypoint.
2. Tighten the portable instructions so Codex and OpenCode usage remains first-class without Claude-specific dependencies.
3. Add the smallest Claude memory setup needed for session dumps into `notes/vault/sessions/`.
4. Document how compaction recovery should read the latest relevant session dump plus the project ledger.
5. Add a lightweight Codex template only if needed for alignment with the shared workspace conventions.
6. Verify the resulting layout and file references after the changes land.

## Risks and Guardrails

- Do not let Claude-specific memory replace `ledger.md`.
- Do not create an OpenCode-specific setup layer unless a real gap appears.
- Do not build a large Codex configuration system before the lightweight workflow proves useful.
- Keep agent-specific setup thin enough that the workspace can still be understood by reading `AGENTS.md` first.
- Do not make Claude memory a prerequisite for the primary Codex or OpenCode workflow.

## Success Criteria

- Codex and OpenCode still operate correctly by reading `AGENTS.md` and the project docs without relying on Claude-only features.
- A new Claude session can recover context from `notes/vault/sessions/` plus the relevant project ledger.
- The workspace has one clear source of truth for shared operating rules.
- Tool guidance clearly prefers `Read` when the file path is already known.

## Implementation Status

- Portable-first Phase 2 setup is in place.
- `AGENTS.md` remains canonical and Codex/OpenCode now have explicit guidance that points back to it.
- `SESSIONS_DIR` now targets `notes/vault/sessions/` and `qmd` is installed.
- Claude plugin installation was deferred after the `agent-utils` marketplace manifest failed validation on the current Claude plugin schema.
