# Run State

This file is the persistent handoff state for Claude Code sessions.

## Current Status

- Target venue: ACML 2026 Conference Track.
- Current phase: **Phase 2 IN PROGRESS** — BCS-50 scoring running.
- Active plan file: `Plan/02_FAIR_BCS_EXPERIMENTS.md`.
- GitHub remote: `git@github.com:Zhe-SH-CN/PGCR.git`.
- Last completed work: Phase 1 + Phase 2a (Direct-10 rejudge) + Phase 2b (BCS-50 generation).
- Current decision: BCS-50 scoring/selection running overnight (~2/77 done as of 23:52 CST).
- External research update: plain candidate expansion is not enough as a novelty claim. Prefer the controlled Sci-Reasoning Hit@10 story, with SE-BCS as a lightweight structured/evolutionary variant if Phase 2 budget allows.
- Skills: Superpowers and planning-with-files confirmed installed by user.

## Phase 2 Progress

### Phase 2a: Direct-10 Enriched Rejudge — COMPLETE
- Script: `scripts/14_direct10_rejudge.py`
- Output: `results/acml_direct10_rejudge_mimo_v25pro.json`
- Result: 77/77 targets, 20 hits, **26.0% Hit@10** (enriched contributions)
- Previous (title-only): 29 hits, 37.7% — enriched judging is stricter
- Tokens: 810,522

### Phase 2b: BCS-50 Generation — COMPLETE
- Script: `scripts/10_vanilla_expansion.py` (5 batches × 10 ideas = 50 candidates/target)
- Output: `results/bcs50_candidates_mimo_v25pro.jsonl`
- Result: 77/77 targets generated
- Last 2 targets used 3 batches instead of 5 due to API timeout issues

### Phase 2c: BCS-50 Scoring/Selection — IN PROGRESS
- Script: `scripts/11_score_and_select_vanilla.py`
- Output: `results/bcs50_selected_mimo_v25pro.jsonl`
- Progress: ~2/77 targets scored as of 23:52 CST
- Expected completion: ~12 hours (overnight run)

### Phase 2d: BCS-50 Evaluation — NOT STARTED
- Script: `scripts/15_bcs_evaluate.py`
- Output: `results/bcs50_eval_mimo_v25pro.json`

## Current Evidence

| Method | Target set | Hits | Total | Hit@10 | 95% CI | Status |
|---|---|---:|---:|---:|---|---|
| Direct-10 rejudge | full | 20 | 77 | 26.0% | — | enriched baseline |
| Direct MiMo v2.5-pro (old) | full | 29 | 77 | 37.7% | [27.3, 48.1] | title-only judging |
| PGCR | full | 22 | 77 | 28.6% | [18.2, 39.0] | negative ablation |
| BCS-50 | full | ? | 77 | ? | — | scoring in progress |

## Phase 1 Completed Work

- Enriched eval data: 77/77 non-empty contributions
- Resume accounting fixed in baseline and eval scripts
- ACML results audit script created
- All Phase 1 exit criteria met

## API Issues Encountered

- MiMo API calls sometimes hang indefinitely (CLOSE-WAIT connections)
- Fixed: added threading-based 300s timeout in `scripts/mimo_client.py`
- Generation calls take 100-300s each (much slower than expected)
- Scoring calls also slow (~100s each)
- Process auto-restart was needed multiple times during BCS-50 generation

## Output Paths

### Phase 1
- `data/scireasoning/eval_neurips_2025_oral_enriched.jsonl`
- `data/scireasoning/enrichment_report.md`
- `scripts/13_acml_results_audit.py`
- `results/acml_results_audit.json`
- `results/acml_results_audit.md`

### Phase 2
- `scripts/14_direct10_rejudge.py`
- `results/acml_direct10_rejudge_mimo_v25pro.json`
- `results/bcs50_candidates_mimo_v25pro.jsonl`
- `results/bcs50_selected_mimo_v25pro.jsonl` (in progress)
- `results/bcs50_eval_mimo_v25pro.json` (not started)

## Next Action

1. Wait for BCS-50 scoring to complete (~12 hours).
2. Run BCS-50 evaluation with enriched contributions.
3. Run the ACML audit script to get updated results table.
4. Reflect on results vs ACML submission bar.
5. If BCS-50 beats Direct-10 by ≥5pp: proceed to SE-BCS-50 and selection ablations.
6. If BCS-50 doesn't beat Direct-10: consider pivot to budget/analysis paper.

## Blockers

- BCS-50 scoring is very slow (~12 hours for 77 targets × ~40 candidates each).
- MiMo API latency is high (100-300s per call for generation, ~100s for scoring).

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
