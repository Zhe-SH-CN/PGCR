# Run State

This file is the persistent handoff state for Claude Code sessions.

## Current Status

- Target venue: ACML 2026 Conference Track.
- Current phase: **Phase 1 COMPLETE — ready for Phase 2 RCPS experiments**.
- Active plan file: `Plan/02_FAIR_BCS_EXPERIMENTS.md` (RCPS portfolio experiments).
- GitHub remote: `git@github.com:Zhe-SH-CN/PGCR.git`.
- Last completed work: Phase 1 protocol repair (2026-06-14).
- Current decision: Proceed to Phase 2 RCPS experiments.
- Skills: Superpowers and planning-with-files confirmed installed by user.

## Phase 1 Completed Work

### 1. Direct-10 Completeness Repair ✅
- Repaired 4 zero-idea targets: Gq4Gay8rDB, m7MD0sa8Re, oJ84bedrtM, zwCb9cKHpd
- All 77 targets now have exactly 10 parsed ideas (770 total)
- Output: `results/direct10_complete_mimo_v25pro.json`
- Report: `results/direct10_repair_report.md`

### 2. Judge Format Audit ✅
- **Critical finding**: MiMo v2.5-pro uses thinking/reasoning tokens (~880 per judge call)
- Old judge with max_tokens=128/256 produced empty responses (0% parse rate)
- New judge with max_tokens=2048 achieves 100% parse rate
- **Label changes**: 11/16 sample judgments changed (new judge is more lenient)
- Output: `results/judge_format_audit.json`
- Report: `results/judge_format_audit.md`

### 3. Scorer/Selector Failure Audit ✅
- BCS scorer: 82.9% fallback scores (overall=3, empty reason)
- PGCR scorer: 98.0% fallback scores
- 76% of BCS selected ideas from batch 0
- Conclusion: Selection not based on meaningful quality scores
- Output: `results/selector_failure_audit.json`
- Report: `results/selector_failure_audit.md`

### 4. Token Cost Audit ✅
- Direct-10: 1,907,440 tokens (24,772/target)
- BCS-50: 4,103,615 tokens (53,294/target) — 2.15x Direct-10
- PGCR: 10,335,016 tokens (134,221/target) — 5.42x Direct-10
- Previous claim "BCS uses only 5% more tokens" was incorrect
- Output: `results/token_cost_audit.json`
- Report: `results/token_cost_audit.md`

### 5. RCPS Results Audit Script ✅
- Computes hits, CIs, paired win/loss, gained/lost IDs, parse failures, token totals
- Output: `scripts/16_rcps_results_audit.py`
- Audit: `results/rcps_results_audit.json`, `results/rcps_results_audit.md`

## Key Findings from Phase 1

1. **MiMo v2.5-pro uses thinking tokens** — judge calls need max_tokens=2048, not 128
2. **Old judge was undercounting hits** — parse failures treated as misses
3. **BCS/PGCR scorers collapsed** — 82-98% fallback scores
4. **Token costs were undercounted** — BCS is 2.15x, not 1.05x

## Current Results (with repaired Direct-10)

| Method | Hits | Hit@10 | Parse Rate | Tokens |
|---|---:|---:|---:|---:|
| Direct-10 (repaired) | 29 | 37.7% | 0.1% | 1,907,440 |
| BCS-50 | 16 | 20.8% | — | 4,103,615 |
| PGCR | 14 | 18.2% | — | 10,335,016 |

Note: Direct-10 hit count (29) uses old judge format with 0.1% parse rate. Need to rejudge with new format.

## Next Action

Execute `Plan/02_FAIR_BCS_EXPERIMENTS.md` for RCPS experiments:

1. Create `results/rcps_preregistered_config.json`
2. Implement `scripts/17_select_rcps_portfolios.py`
3. Implement `scripts/18_evaluate_rcps_portfolios.py`
4. Run RCPS-8/2 and RCPS-5/5 on all 77 targets
5. Run random and diversity-only portfolio ablations

## Blockers

- Need to rejudge Direct-10 with new judge format (max_tokens=2048) for accurate hit counts
- Old hit counts may change significantly under new judge format

## Required Read Order (for next session)

1. `CLAUDE.md`
2. `results/codex_acml_research_audit_2026-06-13.md`
3. `Plan/00_BIG_PLAN.md`
4. `Plan/10_PLAN_AUDIT_ACML_READINESS.md`
5. `Plan/RUN_STATE.md` (this file)
6. `Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md`
7. `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md`
8. `Plan/02_FAIR_BCS_EXPERIMENTS.md`
