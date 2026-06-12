# Operating Guide: Claude Code Automation and Server Operations

## Objective

Let Claude Code run the project from protocol repair to paper draft with minimal user intervention while protecting the shared lab server and MiMo account.

This is an operating guide, not a late-stage phase. Claude Code should read it before Phase 1 and then immediately execute the active phase in `Plan/RUN_STATE.md`.

## Required Read Order

Claude Code should read:

1. `CLAUDE.md`
2. `results/acml_readiness_audit.md`
3. `Plan/00_BIG_PLAN.md`
4. `Plan/10_PLAN_AUDIT_ACML_READINESS.md`
5. `research/current_acml_idea_2026-06-12.md`
6. `research/deep_research_sci_reasoning_2026-06-12.md`
7. `Plan/RUN_STATE.md`
8. this file
9. `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md`
10. active phase plan from `Plan/RUN_STATE.md`

## Server Safety

Rules:

- no `sudo`,
- no system package installation,
- no files outside the project directory,
- no reading `.env`, `.ssh`, private keys, or other users' files,
- no destructive deletion beyond generated caches in this project,
- use `uv` with project-local `.venv` and preferably project-local `.uv-cache`,
- never hardcode or print API keys.

Ask the user only for:

- secrets,
- system-level changes,
- outside-directory access,
- destructive deletion beyond generated caches,
- parallel MiMo execution,
- fine-tuning,
- full KG construction,
- major research-scope changes.

For ordinary missing optional data, minor format ambiguity, or recoverable API failures, use the smallest documented fallback and continue.

## GitHub Remote

Use this repository remote for handoff and backup:

```bash
git@github.com:Zhe-SH-CN/PGCR.git
```

Rules:

- initialize Git only inside the project folder if needed,
- set the remote as `origin`,
- commit and push at every phase or major experiment boundary after exit criteria pass,
- do not commit `.env`, API keys, `.venv`, `.uv-cache`, private keys, secrets, or generated caches,
- run a secret/path scan before pushing,
- do not force-push unless the user explicitly asks.

Major boundaries:

- Phase 1 protocol/data repair,
- Direct-10 enriched rejudge,
- BCS-50 full run,
- SE-BCS-50 full run or skip decision,
- Phase 3 robustness,
- Phase 4 tables/analysis,
- Phase 5 paper draft/compile.

## Python Environment

Use `uv` for all Python dependency work.

Initial setup:

```bash
UV_CACHE_DIR=.uv-cache uv venv .venv
UV_CACHE_DIR=.uv-cache uv pip install -r requirements.txt
```

Rules:

- if `requirements.txt` is absent, create the smallest one needed for the active phase,
- do not use `pip install --user`,
- do not install Conda environments,
- do not modify system Python,
- run scripts with `.venv/bin/python` after setup,
- keep generated caches local when possible.

## MiMo Operations

Environment variables:

```bash
export XIAOMI_MIMO_BASE_URL="..."
export XIAOMI_MIMO_API_KEY="..."
export MIMO_MODEL="mimo-v2.5-pro"
export MIMO_SLEEP_SECONDS="0.5"
```

Rules:

- sleep at least 0.5 seconds between MiMo API calls,
- single worker by default,
- no parallel MiMo calls without user approval,
- on 429/rate-limit errors, back off and resume,
- checkpoint after every target.

## Logging

Every long job must write:

- command arguments,
- timestamp,
- input path,
- output path,
- model,
- judge model,
- prompt version,
- candidate budget,
- sleep seconds,
- target count,
- completed target count,
- failures,
- token usage if available.

Preferred output locations:

- `results/`
- `logs/`
- `paper/tables/`
- `paper/figures/`

## Claude Skills

Check at startup whether these skills are installed. If installed, use them; if missing, record the missing skill in `Plan/RUN_STATE.md` and continue with the local `Plan/` workflow:

- Superpowers: execute-plan, systematic debugging, code review.
- planning-with-files: read-before-decide discipline.

Do not merely mention these skills. Use them for the relevant work:

- Superpowers execute-plan: before each phase or major experiment.
- Superpowers systematic debugging: when scripts fail, outputs conflict, or results are unexpectedly weak.
- Superpowers code review: before phase completion and before commit/push.
- planning-with-files: before major decisions, write or update the relevant `Plan/`, `research/`, or `results/` file instead of relying on chat memory.

## Context Management

At the end of every session:

- update `Plan/RUN_STATE.md`,
- summarize exact output paths,
- record next phase,
- record blockers,
- record reflection against `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md`,
- do not paste huge logs into chat.

When context gets large:

- write findings to files,
- update `Plan/RUN_STATE.md`,
- start a fresh Claude Code session with `Plan/09_CLAUDE_START_PROMPT_ACML.md`.

## Exit Criteria

Automation is healthy if:

- Claude follows the active phase only,
- no permission escalation is needed for ordinary work,
- no MiMo runs lose completed work,
- new sessions can resume from `Plan/RUN_STATE.md`.
