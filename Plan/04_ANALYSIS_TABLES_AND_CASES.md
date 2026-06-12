# Phase 4: Analysis, Tables, and Case Studies

## Objective

Turn experiment outputs into ACML-grade evidence.

No new method claims should be written until this phase has produced the tables and warnings directly from result files.

## Required Tables

### Main Results

File:

- `paper/tables/main_results.tex`

Rows:

- Direct-10 baseline,
- BCS-50,
- SE-BCS-50 if available,
- BCS-100 if available,
- PGCR negative ablation,
- strongest non-oracle ablation.

Columns:

- generator,
- judge,
- candidates generated,
- final ideas,
- hits / total,
- Hit@10,
- delta vs direct baseline,
- token usage.

### Selection Ablations

File:

- `paper/tables/selection_ablation.tex`

Rows:

- random 10,
- score-only,
- diversity-only,
- score + diversity,
- baseline-preserving mixture.

### Robustness

File:

- `paper/tables/robustness.tex`

Rows depend on available results:

- MiMo v2.5 vs v2.5-pro,
- primary judge vs secondary judge,
- optional local open-source subset.

### Cost and Budget Curve

File:

- `paper/tables/budget_curve.tex`

Columns:

- candidate budget,
- Hit@10,
- tokens per target,
- runtime per target,
- marginal gain.

## Statistics

Compute:

- bootstrap confidence interval for each Hit@10,
- paired comparison against direct baseline,
- win/loss/tie target counts,
- exact list of targets gained/lost by BCS.

McNemar's test is useful if easy, but do not overclaim significance on 77 targets.

## Required Case Studies

Create:

- `results/case_studies.md`
- `paper/tables/case_study_index.tex` if useful

Include at least:

- 3 cases where BCS hits and Direct-10 misses,
- 3 cases where BCS loses a Direct-10 hit,
- 3 cases both methods miss,
- 2 cases explaining why PGCR underperforms.

Each case must include:

- target title,
- target contribution,
- predecessor summary,
- Direct-10 best idea,
- BCS selected hit or miss,
- judge reason,
- human-readable explanation.

## Error Analysis

Create:

- `results/error_analysis.md`

Analyze:

- insufficient predecessor information,
- generated ideas too broad,
- selection discards correct direct idea,
- expansion increases diversity but hurts precision,
- structured extraction introduces wrong or over-compressed context,
- crossover/mutation creates plausible but source-bound ideas,
- target depends on hidden author insight,
- judge threshold inconsistency,
- pattern conditioning narrows recall.

## Related-Work Pressure Tests

Create:

- `results/related_work_positioning.md`

Explicitly compare against:

- Graph2Idea: structured graph contexts,
- FlowPIE: MCTS/evolutionary test-time idea search,
- Nova: iterative planning and search,
- LDC: controllable generation through SFT/RL,
- SCI-IDEA: embedding/facet-based ideation metrics,
- RQ-Bench and the human ideation study: LLM judge and novelty-evaluation risks.

For each, state:

- what it does,
- why this project is lighter,
- what was borrowed,
- what was intentionally avoided,
- whether the claim must be weakened.

## Leakage and Integrity Audit

Create:

- `results/protocol_integrity_audit.md`

Check:

- no target title/contribution in generation prompts,
- no target title/contribution in scoring or selection prompts,
- judge sees target only after final ideas are selected,
- no local identifying paths in paper-facing files,
- no API keys in logs or outputs.

## Exit Criteria

Phase 4 is complete when:

- all tables are generated from JSON/JSONL files,
- case studies and error analysis exist,
- related-work positioning exists,
- protocol integrity audit passes,
- the final paper claim is selected based on evidence.
