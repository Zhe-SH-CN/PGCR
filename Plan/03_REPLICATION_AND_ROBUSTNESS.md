# Phase 3: Replication and Robustness

## Objective

Make the result credible enough for ACML by testing whether BCS is robust across models, judges, or local open-source systems.

Do this only after Phase 2 establishes a plausible full-set BCS improvement.

## MiMo Model Replication

This phase is where both MiMo models become part of the evidence package. Phase 2 uses `mimo-v2.5-pro` as the main model; Phase 3 tests whether the conclusion survives on `mimo-v2.5`.

Minimum:

- MiMo v2.5 Direct-10 on all 77 targets.
- MiMo v2.5 BCS-50 on all 77 targets, or a fixed 30-target stratified subset if token/time is tight.

Preferred:

| Generator | Method | Judge | Target set |
|---|---|---|---|
| mimo-v2.5-pro | Direct-10 | mimo-v2.5-pro | 77 |
| mimo-v2.5-pro | BCS-50 | mimo-v2.5-pro | 77 |
| mimo-v2.5 | Direct-10 | mimo-v2.5-pro | 77 |
| mimo-v2.5 | BCS-50 | mimo-v2.5-pro | 77 |

Outputs:

- `results/direct10_mimo_v25_eval.json`
- `results/bcs50_mimo_v25_eval.json`
- updated `results/acml_results_audit.md`

## Judge Robustness

Primary judge:

- MiMo v2.5-pro with enriched target contribution.

Secondary judge options:

- MiMo v2.5.
- local open-source judge on a subset.
- another available API only if credentials already exist.

Minimum judge agreement study:

- sample at least 20 targets or 200 idea judgments,
- judge the same final ideas with two judges,
- compute agreement, both-hit agreement, and disagreement examples.

Outputs:

- `results/judge_agreement_sample.json`
- `results/judge_agreement_report.md`

## Optional Local Open-Source Model Experiments

Use local A6000 first. Use Tencent cnb.cool H20/L40 only if setup is stable and low-friction.

Candidate model families:

- Qwen3 Instruct, 14B or 32B if available.
- DeepSeek-R1-Distill-Qwen, 14B or 32B.
- Llama 3.1/3.3 if weights and serving already exist.

Minimum useful local experiment:

- fixed 30-target stratified subset,
- Direct-10,
- BCS-50,
- same selected final-10 protocol,
- judge with MiMo v2.5-pro or the same local judge.

Do not spend more than one day debugging GPU infrastructure before the main MiMo result and ACML paper are stable.

## Exit Criteria

Phase 3 is complete when one of these is true:

- BCS improvement replicates across MiMo v2.5 and v2.5-pro,
- or a secondary judge supports the qualitative conclusion,
- or local model subset results are available,
- or the failure to replicate is documented and the paper framing is adjusted.
