# Run State

This file is the persistent handoff state for Claude Code sessions.

## Current Status

- Target venue: ACML 2026 Conference Track.
- Current phase: **Phase 1 COMPLETE** — ready for Phase 2.
- Active plan file: `Plan/02_FAIR_BCS_EXPERIMENTS.md`.
- GitHub remote: `git@github.com:Zhe-SH-CN/PGCR.git`.
- Last completed work: Phase 1 protocol and data repair (2026-06-12).
- Current decision: proceed to Phase 2 BCS experiments.
- External research update: plain candidate expansion is not enough as a novelty claim. Prefer the controlled Sci-Reasoning Hit@10 story, with SE-BCS as a lightweight structured/evolutionary variant if Phase 2 budget allows.
- Skills: Superpowers and planning-with-files confirmed installed by user.

## Phase 1 Completed Work

### Task 0: Project-local uv environment
- `.venv/` with Python 3.12.4
- Dependencies: openai, python-dotenv, numpy, scipy (from requirements.txt)
- All imports verified working

### Task 1: Enriched eval data
- Created: `data/scireasoning/eval_neurips_2025_oral_enriched.jsonl` (77 records)
- Created: `data/scireasoning/enrichment_report.md`
- All 77 contributions filled from Sci-Reasoning result files
- Source: `evaluation_results_claude_sonnet_final.json` (primary, 77/77 matched)
- No private paths in output
- Average predecessors: 6.9

### Task 2: Resume accounting fixed
- `scripts/03_run_baseline_mimo.py`: resume now recomputes `total_hits`, `total_input_tokens`, `total_output_tokens` from loaded results
- `scripts/09_evaluate_pgcr.py`: resume now recomputes `total_hits` and `total_tokens` from loaded results
- `results/vanilla_expansion_eval.json`: method field corrected from `pgcr_full` to `vanilla_expansion`

### Task 3: ACML results audit
- Created: `scripts/13_acml_results_audit.py`
- Created: `results/acml_results_audit.json`
- Created: `results/acml_results_audit.md`
- Audit verifies: hits/total/Hit@10, overlaps, oracle warnings, token totals, candidate counts, judge confidence, metadata warnings, bootstrap 95% CI, recomputation checks

## Current Evidence

| Method | Target set | Hits | Total | Hit@10 | 95% CI | Status |
|---|---|---:|---:|---:|---|---|
| Direct MiMo v2.5-pro | full | 29 | 77 | 37.7% | [27.3, 48.1] | baseline, needs enriched rejudge |
| PGCR | full | 22 | 77 | 28.6% | [18.2, 39.0] | negative ablation |
| Vanilla expansion | baseline misses | 16 | 48 | 33.3% | [20.8, 47.9] | diagnostic only |
| Oracle combined | mixed | 45 | 77 | 58.4% | — | not a fair method |

## Key Findings from Audit

- Vanilla expansion was evaluated ONLY on baseline misses (48 targets). Combined results are oracle-style.
- PGCR loses 18 baseline-hit targets (only 11 overlap with baseline hits).
- Judge confidence: hit_mean ~0.84, miss_mean ~0.07 across all methods.
- All recomputation checks pass (reported hits == recomputed hits).

## Phase 1 Exit Criteria — All Met

- ✅ enriched eval file has 77 non-empty contributions
- ✅ robust audit files are generated (`acml_results_audit.json`, `acml_results_audit.md`)
- ✅ known oracle and metadata warnings appear in the audit
- ✅ resume accounting is fixed in baseline and eval scripts
- ✅ `Plan/RUN_STATE.md` updated

## Output Paths

- `data/scireasoning/eval_neurips_2025_oral_enriched.jsonl`
- `data/scireasoning/enrichment_report.md`
- `scripts/13_acml_results_audit.py`
- `results/acml_results_audit.json`
- `results/acml_results_audit.md`

## Next Action

Execute `Plan/02_FAIR_BCS_EXPERIMENTS.md`:

1. Direct-10 enriched rejudge on all 77 targets using enriched contributions.
2. Full-set BCS-50 on all 77 targets (target-hidden generation, scoring, selection).
3. SE-BCS-50 if plain BCS-50 is clean.
4. Selection ablations (random, score-only, diversity-aware).

Before starting Phase 2 MiMo runs, run a 1-3 target smoke test.

## Blockers

None for Phase 2 start.

## Required Read Order (for next session)

1. `CLAUDE.md`
2. `results/acml_readiness_audit.md`
3. `Plan/00_BIG_PLAN.md`
4. `Plan/10_PLAN_AUDIT_ACML_READINESS.md`
5. `research/current_acml_idea_2026-06-12.md`
6. `research/deep_research_sci_reasoning_2026-06-12.md`
7. `Plan/RUN_STATE.md` (this file)
8. `Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md`
9. `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md`
10. `Plan/02_FAIR_BCS_EXPERIMENTS.md`
