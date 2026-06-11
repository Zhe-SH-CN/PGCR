# Supplemental Evidence: Existing MiMo Experiment Artifacts

This file is not the execution plan. The canonical execution plan is `Plan/00_BIG_PLAN.md` plus phases `Plan/01` through `Plan/07`.

Use this file only to understand existing artifacts from the prior Claude Code run.

## Existing Result Files

| Artifact | Path | Status |
|---|---|---|
| Baseline MiMo v2.5-pro | `results/baseline_mimo.json` | full 77 targets |
| PGCR evaluation | `results/pgcr_full.json` | full 77 targets |
| PGCR candidates | `results/pgcr_candidates.jsonl` | full 77 targets |
| PGCR scored | `results/pgcr_scored.jsonl` | full 77 targets |
| PGCR top-10 | `results/pgcr_top10.jsonl` | full 77 targets |
| Vanilla expansion hard cases | `results/vanilla_expansion_hard.jsonl` | 48 baseline misses |
| Vanilla selected hard cases | `results/vanilla_top10_hard.jsonl` | 48 baseline misses |
| Vanilla hard-case evaluation | `results/vanilla_expansion_eval.json` | 48 baseline misses |
| Experiment logs | `logs/experiment_log.jsonl` | useful for token accounting |
| Experiment summary | `logs/experiment_summary.json` | stage summary only |
| Old paper draft | `paper/main.tex` | historical notes only, not ACML source |

## Verified Existing Numbers

| Method | Target set | Hits | Total | Hit@10 | Notes |
|---|---:|---:|---:|---:|---|
| Direct MiMo v2.5-pro | full | 29 | 77 | 37.7% | valid generated ideas, needs enriched rejudge |
| PGCR | full | 22 | 77 | 28.6% | negative ablation |
| Vanilla expansion | baseline misses | 16 | 48 | 33.3% | diagnostic only |
| Oracle combined | mixed | 45 | 77 | 58.4% | not a fair method |

## Known Problems

- The current eval JSONL has empty `contribution` for all 77 targets.
- The old judge therefore used target title as fallback.
- `vanilla_expansion_eval.json` has wrong method metadata.
- Existing markdown summaries contain inconsistent token counts.
- Resume counters in some scripts can be wrong if a run resumes after partial completion.
- Old paper files are not ACML submission sources and contain placeholder citations.

## Useful Local Sources for Enrichment

These local Sci-Reasoning files contain target contributions for title matching:

- `Sci-Reasoning/research_idea_evaluation/results/evaluation_results_claude_sonnet_final.json`
- `Sci-Reasoning/research_idea_evaluation/results/evaluation_results_claude_opus_final.json`
- `Sci-Reasoning/research_idea_evaluation/results/evaluation_results_gemini_25pro_final.json`
- `Sci-Reasoning/research_idea_evaluation/results/evaluation_results_gpt52_v3_exa_final.json`
