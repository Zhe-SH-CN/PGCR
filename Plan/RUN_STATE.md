# Run State

This file is the persistent handoff state for Claude Code sessions.

## Current Status

- Target venue: ACML 2026 Conference Track.
- Current phase: Phase 1, Protocol and Data Repair.
- Active plan file: `Plan/01_PROTOCOL_AND_DATA_REPAIR.md`.
- GitHub remote: `git@github.com:Zhe-SH-CN/PGCR.git`.
- Last completed work: prior Claude Code run produced baseline, PGCR, hard-case expansion, logs, and a non-ACML draft.
- Current decision: follow the ACML/BCS plan only.
- Claude Code starts from Phase 1. `Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md` is a pre-flight operating guide, not a later execution phase.

## Current Evidence

| Method | Target set | Hits | Total | Hit@10 | Status |
|---|---:|---:|---:|---:|---|
| Direct MiMo v2.5-pro | full | 29 | 77 | 37.7% | baseline, needs enriched rejudge |
| PGCR | full | 22 | 77 | 28.6% | negative ablation |
| Vanilla expansion | baseline misses | 16 | 48 | 33.3% | diagnostic only |
| Oracle combined | mixed | 45 | 77 | 58.4% | not a fair method |

## Immediate Problems

- `data/scireasoning/eval_neurips_2025_oral.jsonl` has empty contributions.
- Existing judging used title fallback.
- Full-set BCS has not been run.
- Current `paper/main.tex` draft is not the ACML submission source.
- Current 58.4% number is oracle-style and not a fair main result.

## Required Read Order

1. `CLAUDE.md`
2. `results/acml_readiness_audit.md`
3. `Plan/00_BIG_PLAN.md`
4. `Plan/10_PLAN_AUDIT_ACML_READINESS.md`
5. `Plan/RUN_STATE.md`
6. `Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md`
7. `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md`
8. active phase plan

## Next Action

Execute `Plan/01_PROTOCOL_AND_DATA_REPAIR.md`.

First action: create the project-local `uv` environment from Phase 1 Task 0, then continue with data repair.

Do not start long MiMo experiments until Phase 1 audit outputs prove the enriched data and summarizer are correct.
