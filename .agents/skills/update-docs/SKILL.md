# Skill: update-docs

Use this skill after a meaningful feature, milestone, or integration lands.

## Goal

Keep project docs aligned with the real state of the codebase.

## Update Order

Review and update the following files as needed:

1. `README.md` for user-visible behavior, setup, or usage changes
2. `docs/plan.md` for milestone status, priorities, or scope changes
3. `docs/resources.md` for new commands, integrations, file references, or external docs
4. `ledger.md` for the dated handoff record of what changed and what is next

## Workflow

1. Identify what actually changed: capability, setup, architecture, external services, or operator workflow.
2. Update only the docs affected by that change.
3. Keep entries short, specific, and truthful.
4. Call out new env vars, migrations, validation steps, or operational risks when they matter.
5. Prefer explaining why the change matters rather than listing filenames without context.

## Guardrails

- Do not paste large diffs into docs.
- Do not promise follow-up work that is not agreed on.
- Do not leave docs in a half-updated state; if one file changes the meaning of another, update both.

## Completion

Tell the user which docs were updated and whether any project docs are still missing.
