# Run State

This file is the persistent handoff state for Claude Code sessions.

## Current Status

- Target venue: ACML 2026 Conference Track.
- Current phase: **Phase 2 COMPLETE — CRITICAL NEGATIVE RESULT**.
- Active plan file: `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md` (replanning triggered).
- GitHub remote: `git@github.com:Zhe-SH-CN/PGCR.git`.
- Last completed work: Phase 2 BCS-50 full evaluation (2026-06-13).
- Current decision: **REPLAN TRIGGERED** — BCS-50 does not beat Direct-10.
- Skills: Superpowers and planning-with-files confirmed installed by user.

## Phase 2 Final Results

### Main Results Table (enriched judging)

| Method | Target set | Hits | Total | Hit@10 | Tokens | Status |
|---|---|---:|---:|---:|---:|---|
| Direct-10 rejudge (enriched) | full | 20 | 77 | 26.0% | 810,522 | baseline |
| BCS-50 (score+diversity) | full | 16 | 77 | 20.8% | 853,688 | main method |
| PGCR | full | 22 | 77 | 28.6% | — | old judging, needs rejudge |
| MiMo v2.5-pro (old title-only) | full | 29 | 77 | 37.7% | — | title-only judging |

### Critical Finding

**BCS-50 (20.8%) does NOT beat Direct-10 (26.0%).** BCS-50 is 5.2pp LOWER than the baseline.

This directly triggers the replanning mechanism per `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md`:
> "Replan if: BCS-50 does not beat Direct-10"

### Replanning Analysis

**Trigger:** BCS-50 does not beat Direct-10 on the full 77-target set.

**Evidence:**
- Direct-10 enriched: 20/77 = 26.0%
- BCS-50: 16/77 = 20.8%
- BCS-50 is 5.2pp WORSE than Direct-10

**Decision:** Per `Plan/07`, this triggers pivot or stop.

**Insufficiency/Risk:**
- The core BCS hypothesis (more candidates → better Hit@10) is not supported by the data.
- PGCR was already negative (28.6% with old judging, but needs enriched rejudge to confirm).
- BCS-50 is also negative.
- SE-BCS-50 could potentially help (structured/evolutionary approach), but the base BCS result suggests the problem may be deeper than candidate diversity.
- The enriched judging is stricter (26.0% vs 37.7% title-only), which is itself a finding worth reporting.

### BCS-50 vs Direct-10 Overlap Analysis

**Key finding: BCS-50 and Direct-10 find mostly DIFFERENT targets.**

| Metric | Value |
|---|---|
| Direct-10 hits | 20 |
| BCS-50 hits | 16 |
| Common hits | 5 |
| Only in Direct-10 | 15 |
| Only in BCS-50 | 11 |
| BCS-50 hits on Direct-10 misses | 11 |
| BCS-50 loses Direct-10 hits | 15 |

**Interpretation:**
- Only 5 out of 31 total hits overlap between the two methods.
- BCS-50 finds 11 targets that Direct-10 misses, but loses 15 that Direct-10 hits.
- This means candidate expansion substantially changes which ideas are generated.
- But the net effect is negative: the loss of 15 Direct-10 hits outweighs the 11 new hits.
- Judge confidence is nearly identical: hit_mean ~0.868 for both methods.

**Implication for paper:**
- The finding that BCS and Direct find different targets is interesting and reportable.
- It suggests that more candidates don't just add noise — they shift the entire distribution.
- But the shift is not consistently beneficial.
- This is a strong negative-result finding for an analysis paper.

**Rollback condition:** If SE-BCS-50 also fails, the project should pivot to a negative-result / analysis paper.

**Decision:** Try SE-BCS-50 as a last attempt. If it also fails, pivot to negative-result paper.

**Rollback condition:** If SE-BCS-50 does not beat Direct-10 by ≥5pp, pivot to:
> "When More Ideas Do Not Help: Failure Modes of Candidate Expansion for Scientific Ideation"

**Next action:**
1. Implement SE-BCS-50 (structured predecessor extraction + crossover/mutation + anti-mirage selection).
2. Run SE-BCS-50 on all 77 targets.
3. If SE-BCS-50 beats Direct-10: proceed with improvement paper.
4. If SE-BCS-50 does not beat Direct-10: pivot to negative-result paper with enriched judging + BCS/Direct overlap analysis.

## Possible Pivot Framing

If BCS and SE-BCS both fail, the paper becomes:

**Title:** "When More Ideas Do Not Help: Failure Modes of Candidate Expansion for Scientific Ideation"

**Story:**
- We test whether generating more candidates and selecting quality/diversity improves Sci-Reasoning Hit@10.
- Under enriched judging, direct generation (26.0%) outperforms BCS-50 (20.8%).
- Pattern-conditioned generation (PGCR) also fails.
- Enriched judging is significantly stricter than title-only judging (26.0% vs 37.7%).
- This reveals fundamental limitations of candidate expansion for scientific ideation.

This is still ACML-worthy if the analysis is clean and the negative result is well-structured.

## Phase 1 Completed Work

- Enriched eval data: 77/77 non-empty contributions
- Resume accounting fixed in baseline and eval scripts
- ACML results audit script created
- All Phase 1 exit criteria met

## Phase 2 Completed Work

- Direct-10 enriched rejudge: 77/77, 20 hits, 26.0%
- BCS-50 generation: 77/77 targets, 50 candidates each
- BCS-50 scoring/selection: 77/77 targets
- BCS-50 evaluation: 77/77, 16 hits, 20.8%

## Output Paths

### Phase 1
- `data/scireasoning/eval_neurips_2025_oral_enriched.jsonl`
- `data/scireasoning/enrichment_report.md`
- `scripts/13_acml_results_audit.py`
- `results/acml_results_audit.json`
- `results/acml_results_audit.md`

### Phase 2
- `scripts/14_direct10_rejudge.py`
- `results/acml_direct10_rejudge_mimo_v25pro.json` (20 hits, 26.0%)
- `results/bcs50_candidates_mimo_v25pro.jsonl` (77/77)
- `results/bcs50_selected_mimo_v25pro.jsonl` (77/77)
- `results/bcs50_eval_mimo_v25pro.json` (16 hits, 20.8%)

## Next Action

1. Run ACML audit script to update results table.
2. Analyze BCS-50 vs Direct-10 overlap: which targets does BCS help/hurt?
3. Decide: SE-BCS-50 attempt, or pivot to negative-result paper.
4. Update `Plan/RUN_STATE.md` with decision.
5. Commit and push.

## Blockers

- BCS-50 does not beat Direct-10 — replanning required.
- ACML deadline is 2026-06-26 AoE (13 days from now).
- Need to decide quickly: SE-BCS-50 attempt or pivot.

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
