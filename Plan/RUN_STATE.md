# Run State

This file is the persistent handoff state for Claude Code sessions.

## Current Status

- Current phase: Phase 3 (PGCR) — IN PROGRESS with replan
- Last completed phase: Phase 2 (Baseline)
- Active plan file: `Plan/03_METHOD_PGCR.md` (revised)
- Best known baseline Hit@10: 37.7% (29/77)
- Best known PGCR Hit@10: 0/3 on smoke test (concerning)
- Current decision: Replan triggered — pivot PGCR strategy

## Replan Event 2026-06-10

**Trigger:** PGCR underperforms baseline on smoke test (0/3 vs 2/3).

**Evidence:**
- Baseline generates broad ideas that match judge's semantic matching (confidence 0.85)
- PGCR pattern-conditioned ideas are more creative but narrower
- The judge rewards general direction match, not pattern-specific innovation
- 2 targets baseline hit, PGCR missed both

**Affected phase:** Phase 3 (PGCR method) and Phase 4 (experiments)

**Options considered:**
1. Continue full PGCR pipeline (77 targets, ~53 hours) — risky if PGCR doesn't improve
2. Reduce to top-3 patterns only — cuts calls by 60%
3. Hard-case-only PGCR — apply PGCR only to targets baseline missed (~48 targets)
4. Pivot paper framing — "analysis of MiMo ideation failure modes"
5. Generate more candidates with vanilla prompt (no patterns) and rerank

**Decision:** Option 3 (hard-case-only PGCR) + Option 5 (vanilla expansion baseline)
- Run vanilla MiMo with 50-100 candidates per target (no patterns, just repeated generation)
- Run PGCR only on the ~48 targets baseline missed
- Compare: vanilla-10, vanilla-50, PGCR-hard-cases
- This tests whether the improvement comes from more candidates OR from pattern guidance

**Rollback condition:** If hard-case PGCR also underperforms vanilla expansion, pivot to "failure analysis" paper.

**Next action:** 
1. Let current PGCR candidate generation finish (for data)
2. Create vanilla expansion script (repeated generation, no patterns)
3. Evaluate both on hard cases only
4. Decide based on results

## Phase 2 Results

- Baseline: 37.7% Hit@10 (29/77)
- Total tokens: 1,096,918
- Completed: 77/77 targets
- Results: `results/baseline_mimo.json`
- Summary: `results/baseline_summary.md`

## Phase 3 Progress

- PGCR candidates generated: 17/77 (in progress)
- PGCR smoke test: 0/3 hits (concerning)
- Silent pattern failure bug fixed in script

## Paths to Outputs

| Output | Path |
|--------|------|
| Baseline results | `results/baseline_mimo.json` |
| Baseline summary | `results/baseline_summary.md` |
| PGCR candidates (partial) | `results/pgcr_candidates.jsonl` |
| PGCR smoke eval | `results/pgcr_smoke_eval.json` |
| Experiment log | `logs/experiment_log.jsonl` |
| Experiment summary | `logs/experiment_summary.json` |

## Known Constraints

- ICTAI deadline: 2026-06-30 (20 days remaining)
- MiMo rate limit: 0.5s sleep between calls, shared with Claude Code
- Token budget: baseline used ~1.1M tokens, PGCR full would use ~15M
- No parallel MiMo calls without user approval

## Next Action

1. Create vanilla expansion script (50-100 candidates, no patterns)
2. Run on hard cases (baseline misses)
3. Compare PGCR vs vanilla expansion on same targets
4. Decide paper framing based on results
