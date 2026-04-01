# Agent Workspace

Agent Workspace is a reusable starter template for running software projects with Codex, OpenCode, and Claude-compatible instruction files. It draws mostly from [agent-utils](https://github.com/k3nnethfrancis/agent-utils/) and [ECC](https://ecc.tools/).

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

