# Phase 4 Plan: Experiments and Ablations

## Objective

Run enough experiments to support or reject the PGCR paper claim.

The goal is not to run every possible prompt. The goal is to produce a compact, credible result table for an ICTAI 8-page paper.

## Main Experiment

Evaluate the following methods on the same target set and same judge:

| Method | Candidate Count | Selection |
|---|---:|---|
| Vanilla MiMo | 10 | direct output |
| MiMo More Samples | 100 | simple top/raw/random depending implementation |
| Pattern Generation Only | 80-120 | no reranker or simple per-pattern top |
| PGCR Full | 80-120 | pattern-aware score + diversity selection |

Primary metric:

- Hit@10

Secondary metrics:

- average candidates generated
- valid parse rate
- average judge confidence
- token usage
- runtime
- cost proxy if token pricing is known

## Ablations

Minimum ablations:

1. No pattern prompts: generate many candidates with vanilla prompt.
2. No reranker: pattern candidates but random or simple selection.
3. No diversity penalty: top 10 by score only.
4. Top-3 patterns only: use dominant Sci-Reasoning patterns.
5. Recipes only: use only pattern combinations.

Optional ablations:

- Candidate count: 20 / 50 / 100 / 160.
- Judge model: MiMo vs another judge.
- Context: titles only vs titles + relationship sentences + synthesis narrative.

## Experiment Protocol

Each run must have:

- unique run ID
- timestamp
- model name
- `sleep_seconds`, defaulting to 0.5 for every MiMo API call
- prompt template version
- dataset manifest hash or file path
- target count
- random seed if randomness is used
- raw outputs
- parsed outputs
- judge outputs
- summary metrics

MiMo execution constraints:

- Use single-worker MiMo execution by default.
- Sleep at least 0.5 seconds between MiMo calls.
- Resume from checkpoints after 429/rate-limit errors.
- Only increase concurrency if the user explicitly approves it after reviewing a stable single-worker run.

Suggested output naming:

```text
results/runs/
  2026-06-XX_baseline_mimo/
  2026-06-XX_pgcr_full_v1/
  2026-06-XX_ablation_no_diversity/
```

## Statistical Reporting

For 77 targets, report:

- Hit@10 as hits / total and percentage.
- Bootstrap confidence interval if easy.
- Paired comparison against baseline if the same targets are used.

Do not overclaim statistical significance if the sample is small.

## Error Analysis

Create:

- `results/error_analysis.md`

Include:

- cases baseline missed but PGCR hit
- cases baseline hit but PGCR missed
- cases both missed
- common failure patterns:
  - target depends on hidden author insight
  - predecessor context insufficient
  - generated ideas too broad
  - judge too strict/too lenient
  - duplicates crowd out diversity

## Case Studies

Save at least 3 successful case studies:

```markdown
## Case: target paper title

Predecessor summary:

Baseline output:

PGCR selected idea:

Judge reason:

Why PGCR helped:
```

Save at least 3 failure cases with honest analysis.

## Decision Rules

### Submit

Submit if:

- PGCR beats vanilla MiMo by at least 5 percentage points, or
- PGCR has smaller improvement but strong case studies and ablations.

### Weak Submit

Consider submitting if:

- PGCR improves only 2-5 percentage points, but the method is clean and ablations support the mechanism.

### Pivot

Pivot if:

- PGCR does not beat vanilla MiMo.

Allowed pivots:

1. Emphasize candidate expansion and diversity, not patterns.
2. Use pattern prompts only for hard cases found by baseline.
3. Change from "improves Hit@10" to "an analysis of MiMo scientific ideation failure modes" only if results are interesting.

### Stop

Stop if:

- eval data cannot be reconstructed,
- judge is unusably unstable,
- no method beats baseline after reasonable prompt/rerank fixes.

## Deliverables

- `results/main_results.md`
- `results/main_results.json`
- `results/ablation_results.md`
- `results/ablation_results.json`
- `results/error_analysis.md`
- `results/case_studies.md`
- Updated `Plan/RUN_STATE.md`
