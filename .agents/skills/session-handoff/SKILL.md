# Skill: session-handoff

Use this skill before ending a work session or compacting context.

## Goal

Leave the project in a state where any agent can resume quickly with minimal guesswork.

## Required Outputs

- `ledger.md` updated with the newest session entry at the top
- `docs/plan.md` updated if priorities or milestones changed
- `docs/resources.md` updated if commands, references, or integrations changed
- a concise user-facing handoff summary

## Workflow

1. Read `ledger.md`, `docs/plan.md`, and `docs/resources.md`.
2. Review what changed in the session.
3. Update `ledger.md` with a new dated entry at the top that captures:
   - what was completed
   - important decisions or constraints
   - blockers or open questions
   - the most useful next step
4. Update `docs/plan.md` if the current milestone, scope, or priorities changed.
5. Update `docs/resources.md` if the session introduced new commands, files, services, or references worth keeping.
6. Keep the notes factual and specific. Prefer file paths, outcomes, and decisions over long prose.

## Guardrails

- Do not delete or rewrite older ledger entries unless the user asks.
- Do not claim work is complete unless the code or docs actually reflect it.
- If nothing meaningful changed, add a minimal ledger note or state clearly that no doc update was needed.

## Completion

Tell the user which files were updated and name the next recommended step.
