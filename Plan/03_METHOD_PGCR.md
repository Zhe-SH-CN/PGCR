# Phase 3 Plan: PGCR Method Implementation

## Objective

Implement the proposed lightweight method:

**PGCR: Pattern-Guided Candidate Expansion and Reranking.**

This phase should produce a working method before any full-scale experiment.

## Method Components

### Component A: Pattern-conditioned generation

Use the following patterns and recipes:

1. Gap-Driven Reframing
2. Cross-Domain Synthesis
3. Representation Shift
4. Data & Evaluation Engineering
5. Formal-Experimental Tightening
6. Gap-Driven Reframing + Representation Shift
7. Cross-Domain Synthesis + Representation Shift
8. Gap-Driven Reframing + Cross-Domain Synthesis

For each target, ask MiMo to generate 10-15 ideas per pattern/recipe.

Total candidate target:

- Minimum: 60 candidates
- Preferred: 80-120 candidates
- Maximum: 160 candidates unless ablation needs more

MiMo call pacing:

- Default to `--sleep-seconds 0.5`.
- Sleep at least 0.5 seconds between generation, reranking, and judging calls.
- Do not run parallel MiMo workers unless the user explicitly approves it.
- On 429/rate-limit errors, back off and resume from the latest checkpoint.

### Component B: Candidate normalization

Each idea must be normalized to:

```json
{
  "candidate_id": "string",
  "target_id": "string",
  "pattern": "string",
  "title": "string",
  "description": "string",
  "predecessor_links": ["string"],
  "raw_output": "string"
}
```

### Component C: Pattern-aware scoring

Score every candidate from 1 to 5 on:

- `grounding`: clearly builds on predecessors
- `pattern_fit`: matches assigned pattern
- `specificity`: concrete technical contribution
- `plausibility`: could plausibly be a top AI paper
- `novelty`: not merely restating predecessors
- `clarity`: concise and understandable

Optional score:

- `target_likelihood`: likely to match a real target paper, without seeing target title

Do not leak target title or target contribution into reranking prompts.

### Component D: Diversity-aware Top-10 selection

Use a simple deterministic selection:

1. Sort candidates by weighted score.
2. Iteratively add the best candidate.
3. Penalize near-duplicates of already selected candidates.
4. Keep pattern diversity when possible.
5. Stop at 10.

The selector does not need embeddings at first. Start with LLM duplicate judgments or simple text overlap. Add embeddings only if duplicate ideas are a real problem.

## Scripts to Create

### `scripts/06_generate_pgcr_candidates.py`

Arguments:

- `--input`
- `--output results/pgcr_candidates.jsonl`
- `--model mimo-v2.5-pro`
- `--candidates-per-pattern 12`
- `--limit`
- `--resume`

Outputs one JSON object per target with all candidates.

### `scripts/07_score_pgcr_candidates.py`

Arguments:

- `--input results/pgcr_candidates.jsonl`
- `--output results/pgcr_scored.jsonl`
- `--model mimo-v2.5-pro`
- `--resume`

Output includes scoring JSON and raw reranker output.

### `scripts/08_select_pgcr_top10.py`

Arguments:

- `--input results/pgcr_scored.jsonl`
- `--output results/pgcr_top10.jsonl`

This script should be deterministic and should not call MiMo.

### `scripts/09_evaluate_pgcr.py`

Arguments:

- `--input results/pgcr_top10.jsonl`
- `--output results/pgcr_full.json`
- `--judge-model mimo-v2.5-pro`
- `--resume`

Use the same judge implementation as baseline.

## Prompt Design

### Pattern prompt template

The prompt should contain:

- predecessor list
- role/relationship info
- pattern definition
- positive constraints
- output schema

The prompt should not contain:

- target title
- target contribution
- target abstract

### Reranker prompt template

The prompt should ask MiMo to judge candidates only by:

- quality relative to predecessor information
- pattern adherence
- specificity
- diversity signals

It must not be told the target paper.

## First Smoke Test

Run on 3 targets:

1. Generate candidates.
2. Score candidates.
3. Select Top-10.
4. Evaluate Top-10.
5. Compare with baseline for the same 3 targets.

Do not tune on the full eval set before the pipeline is stable.

## Success Criteria

Phase 3 is complete when:

- 3-paper smoke test completes end to end.
- Candidate count per target is within target range.
- At least 8 of final Top-10 ideas are non-duplicates.
- Outputs are valid JSON.
- Runtime and token usage are acceptable for full run.

## Complexity Budget

Keep this phase small.

Allowed:

- prompt changes
- scoring-weight changes
- simple diversity penalty
- JSON repair for model outputs

Avoid:

- full graph databases
- neural rerankers
- fine-tuning
- heavyweight PDF ingestion
- large refactors of original Sci-Reasoning repo

## Deliverables

- `results/pgcr_candidates_sample.jsonl`
- `results/pgcr_scored_sample.jsonl`
- `results/pgcr_top10_sample.jsonl`
- `results/pgcr_sample_eval.json`
- Updated `Plan/RUN_STATE.md`
