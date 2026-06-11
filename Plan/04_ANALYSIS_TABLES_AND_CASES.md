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
- target depends on hidden author insight,
- judge threshold inconsistency,
- pattern conditioning narrows recall.

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
- protocol integrity audit passes,
- the final paper claim is selected based on evidence.

