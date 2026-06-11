# Phase 2: Fair BCS Experiments

## Objective

Test Budgeted Candidate Search under a fair, full-set, oracle-free protocol.

This phase decides whether the paper can be an improvement paper.

Primary model for this phase:

- `mimo-v2.5-pro`.

`mimo-v2.5` is intentionally deferred to Phase 3 replication/robustness so Phase 2 can first establish whether the method is worth scaling.

## Main Methods

All methods must use the same 77 targets and same enriched judge inputs.

| Method | Candidate generation | Final ideas | Use |
|---|---:|---:|---|
| Direct-10 | 10 | 10 | baseline |
| BCS-50 score+diversity | 50 | 10 | main method |
| BCS-100 score+diversity | 100 | 10 | budget curve, if time permits |
| PGCR full | about 80 | 10 | negative ablation |
| Random-10 from expanded pool | 50 or 100 | 10 | selection ablation |
| Score-only top 10 | 50 or 100 | 10 | selection ablation |
| Diversity-only top 10 | 50 or 100 | 10 | selection ablation |
| Baseline-preserving mixture | 50 plus direct | 10 | optional safety ablation |

Do not use baseline hit/miss labels during generation, scoring, selection, or allocation.

## Direct-10 Rejudge

Use existing generated ideas from:

- `results/baseline_mimo.json`

Rejudge them with:

- `data/scireasoning/eval_neurips_2025_oral_enriched.jsonl`
- fixed judge prompt
- fixed judge model

Output:

- `results/acml_direct10_rejudge_mimo_v25pro.json`

Purpose:

- replaces title-only/fallback judging,
- gives the correct baseline for ACML tables.

## BCS-50 Full

Use or extend `scripts/10_vanilla_expansion.py`, but remove hard-case-only filtering for the main run.

Required outputs:

- `results/bcs50_candidates_mimo_v25pro.jsonl`
- `results/bcs50_selected_mimo_v25pro.jsonl`
- `results/bcs50_eval_mimo_v25pro.json`

Protocol:

1. Generate 5 batches of 10 ideas for every target.
2. Score all candidates with target-hidden criteria.
3. Select exactly 10 ideas per target.
4. Judge selected ideas against target title and enriched contribution.
5. Store raw output, parsed candidates, selected ideas, judgments, and token usage.

Default generation:

- model: `mimo-v2.5-pro`
- sleep: at least 0.5 seconds
- single worker
- resume enabled

## BCS-100 Full

Run only if BCS-50 is promising or a budget curve is needed.

Outputs:

- `results/bcs100_candidates_mimo_v25pro.jsonl`
- `results/bcs100_selected_mimo_v25pro.jsonl`
- `results/bcs100_eval_mimo_v25pro.json`

Stop early if:

- BCS-50 does not improve over direct baseline and error analysis shows expansion mostly loses baseline hits,
- MiMo rate limits become disruptive,
- paper timeline becomes the limiting factor.

## Selection Ablations

Minimum ablations:

- random 10 from BCS-50 candidate pool,
- score-only top 10,
- score + diversity,
- baseline-preserving mixture with fixed rule.

Baseline-preserving mixture examples:

- keep first 5 Direct-10 ideas and fill 5 from BCS selected pool,
- keep top 3 Direct-10 ideas by target-hidden score and fill 7 from BCS selected pool.

The mixture rule must be predeclared and target-hidden.

## Metrics

Primary:

- Hit@10.

Secondary:

- hits / total,
- paired win/loss vs direct baseline,
- candidate parse rate,
- average candidates per target,
- token usage,
- runtime,
- judge confidence,
- duplicate rate,
- selection diversity.

## Exit Criteria

Phase 2 is complete when:

- full-set BCS-50 result exists,
- direct baseline is rejudged with enriched contributions,
- at least two selection ablations exist,
- `results/acml_results_audit.md` contains the updated main table,
- `Plan/RUN_STATE.md` records whether the project passes the ACML improvement bar.
