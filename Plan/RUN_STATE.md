# Run State

This file is the persistent handoff state for Claude Code sessions.

## Current Status

- Current phase: Phase 5 (Paper Writing) — IN PROGRESS
- Last completed phase: Phase 4 (Experiments)
- Active plan file: `Plan/05_PAPER_WRITING.md`
- Best known baseline Hit@10: 37.7% (29/77, 10 candidates)
- Best known PGCR Hit@10: 28.6% (22/77, pattern-guided, ~80 candidates)
- Best known vanilla expansion Hit@10: 33.3% (16/48, hard cases, 50 candidates)
- Best known combined Hit@10: 58.4% (45/77)
- Current decision: Paper draft complete, ready for final review

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

## Final Experiment Results

### Main Results

| Method | Candidates | Hit@10 | Improvement |
|--------|-----------|--------|-------------|
| Vanilla MiMo | 10 | 37.7% (29/77) | — |
| PGCR (pattern-guided) | ~80 | 28.6% (22/77) | -9.1pp |
| Vanilla Expansion (hard cases) | 50 | 33.3% (16/48) | — |
| **Combined** | 10+50 | **58.4% (45/77)** | **+20.7pp** |

### Key Finding

**Pattern conditioning HURTS performance.** PGCR (28.6%) < Baseline (37.7%). Simple candidate expansion HELPS performance. Combined (58.4%) >> Baseline (37.7%).

### Pattern Predictability

| Pattern | Hits | Total | Hit@10 |
|---------|------|-------|--------|
| Representation Shift | 6 | 12 | 50.0% |
| Formal-Experimental | 5 | 12 | 41.7% |
| Gap-Driven Reframing | 6 | 18 | 33.3% |
| Cross-Domain Synthesis | 5 | 17 | 29.4% |

### Token Usage

| Method | Tokens | Targets |
|--------|--------|---------|
| Baseline | 1.1M | 77 |
| PGCR Generation | ~2M | 77 |
| PGCR Scoring | ~5M | 77 |
| Vanilla Expansion | 1.0M | 48 |
| Vanilla Scoring | 4.2M | 48 |

## Phase 3 Progress

- PGCR candidates generated: 17/77 (in progress)
- PGCR smoke test: 0/3 hits (concerning)
- Silent pattern failure bug fixed in script

## Paths to Outputs

| Output | Path |
|--------|------|
| Baseline results | `results/baseline_mimo.json` |
| Baseline summary | `results/baseline_summary.md` |
| PGCR candidates | `results/pgcr_candidates.jsonl` |
| PGCR scored | `results/pgcr_scored.jsonl` |
| PGCR top-10 | `results/pgcr_top10.jsonl` |
| PGCR evaluation | `results/pgcr_full.json` |
| Vanilla expansion | `results/vanilla_expansion_hard.jsonl` |
| Vanilla scored | `results/vanilla_scored_hard.jsonl` |
| Vanilla evaluation | `results/vanilla_expansion_eval.json` |
| Paper draft | `paper/main.tex` |
| References | `paper/references.bib` |
| Submission checklist | `paper/submission_checklist.md` |
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
