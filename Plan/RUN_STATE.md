# Run State

This file is the persistent handoff state for Claude Code sessions.

## Current Status

- Target venue: ACML 2026 Conference Track.
- Current phase: Phase 1 COMPLETE. Ready for Phase 2.
- Active plan file: `Plan/02_FAIR_BCS_EXPERIMENTS.md`.
- GitHub remote: `git@github.com:Zhe-SH-CN/PGCR.git`.
- Last completed work: Phase 1 protocol and data repair (2026-06-11).
- Current decision: follow the ACML/BCS plan only.
- Git initialized with initial commit on `main` branch.

## Current Evidence

| Method | Target set | Hits | Total | Hit@10 | 95% CI | Status |
|---|---:|---:|---:|---:|---|---|
| Direct MiMo v2.5-pro | full | 29 | 77 | 37.7% | [27.3%, 48.1%] | baseline |
| PGCR | full | 22 | 77 | 28.6% | [18.2%, 39.0%] | negative ablation |
| Vanilla expansion | baseline misses | 16 | 48 | 33.3% | [20.8%, 47.9%] | hard-case only |
| Oracle combined | mixed | 45 | 77 | 58.4% | — | NOT FAIR |

## Phase 1 Completed Work

- [x] Git initialized, remote set to `git@github.com:Zhe-SH-CN/PGCR.git`, initial commit made.
- [x] `.venv` created with `uv`, dependencies installed: openai, python-dotenv, numpy, scipy.
- [x] `data/scireasoning/eval_neurips_2025_oral_enriched.jsonl` — 77 records, 77 non-empty contributions, sourced from `evaluation_results_claude_opus_final.json`.
- [x] `data/scireasoning/enrichment_report.md` — validates 77/77 match, avg 6.9 predecessors, no path leaks.
- [x] Resume accounting fixed in `scripts/03_run_baseline_mimo.py` and `scripts/09_evaluate_pgcr.py` — counters recomputed from loaded results on resume.
- [x] `scripts/13_acml_results_audit.py` — robust JSON-based audit script.
- [x] `results/acml_results_audit.json` — machine-readable audit.
- [x] `results/acml_results_audit.md` — human-readable audit with warnings.
- [x] `.gitignore` updated: excludes .venv, .uv-cache, Sci-Reasoning/, raw/, logs/.

## Known Warnings (carried forward)

- `vanilla_expansion_eval.json` has `method: "pgcr_full"` — metadata mismatch flagged in audit.
- Vanilla expansion evaluated on baseline-miss subset only — not a fair full-set result.
- Existing judging used title-only fallback (contributions now enriched for future rejudge).
- Current `paper/main.tex` is IEEE format, not ACML — must use `ACML_camera_ready/` template.

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

Execute `Plan/02_FAIR_BCS_EXPERIMENTS.md`.

Phase 2 runs fair full-set BCS experiments:
- BCS-50 on all 77 targets with target-hidden generation/scoring/selection.
- Enriched rejudge of direct baseline on enriched contributions.
- Selection ablations (random, quality-only, diversity-only, quality+diversity).

Before any long MiMo run, run a 1-3 target smoke test.
