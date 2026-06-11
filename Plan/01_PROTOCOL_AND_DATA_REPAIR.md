# Phase 1: Protocol and Data Repair

## Objective

Make the project trustworthy before spending more MiMo tokens.

This phase should use local files only unless a missing fact cannot be recovered locally. Prefer no API calls in Phase 1.

## Inputs

- `data/scireasoning/eval_neurips_2025_oral.jsonl`
- `Sci-Reasoning/research_idea_evaluation/results/*.json`
- `results/baseline_mimo.json`
- `results/pgcr_full.json`
- `results/vanilla_expansion_eval.json`
- `logs/experiment_summary.json`
- `results/acml_readiness_audit.md`
- `ACML_camera_ready/`

## Tasks

### 0. Create the local Python environment

Use `uv` only. Do not install system packages.

Preferred commands:

```bash
UV_CACHE_DIR=.uv-cache uv venv .venv
UV_CACHE_DIR=.uv-cache uv pip install -r requirements.txt
```

If `requirements.txt` is missing, create the smallest dependency file required for Phase 1 scripts, then install it with `uv pip`.

Validation:

- `.venv/bin/python --version` works.
- project scripts are run with `.venv/bin/python`.
- no dependency installation writes outside this project except normal user-level `uv` behavior.

### 1. Create enriched eval data

Create:

- `data/scireasoning/eval_neurips_2025_oral_enriched.jsonl`
- `data/scireasoning/enrichment_report.md`

Requirements:

- Preserve all 77 target records.
- Fill every `contribution` field.
- Add `contribution_source`.
- Match by normalized title.
- Prefer local Sci-Reasoning result files with complete `contribution` fields.
- Preserve predecessor information.
- Remove or normalize machine-specific source paths before paper-facing outputs.

Validation:

- 77 records.
- 77 non-empty contributions.
- no `/home/`, `/Users/`, API key, or local username in paper-facing fields.
- average predecessor count still around 6-7.

### 2. Repair result metadata

Create or update scripts so future outputs contain:

- method
- model
- judge model
- candidate budget
- prompt template version
- input data path
- sleep seconds
- run ID
- timestamp
- total targets
- completed targets
- hits
- total input/output tokens
- failures

Fix the known metadata issue:

- `results/vanilla_expansion_eval.json` currently reports `method: pgcr_full`; future audits must flag or correct this in derived summaries.

### 3. Fix resume accounting

Fix or wrap:

- `scripts/03_run_baseline_mimo.py`
- `scripts/09_evaluate_pgcr.py`
- any new BCS evaluation script

Resume mode must recompute completed hits and token totals from loaded result files before processing remaining targets.

Do not trust counters initialized to zero after loading existing results.

### 4. Build robust ACML summarizer

Create:

- `scripts/12_acml_results_audit.py`
- `results/acml_results_audit.json`
- `results/acml_results_audit.md`

The summarizer must read JSON/JSONL result files directly and compute:

- hits / total / Hit@10
- overlaps between baseline, PGCR, and expansion
- which results are full-set vs hard-case-only
- oracle-result warnings
- token totals from result files and logs
- candidate count distribution
- judge confidence summary
- missing metadata warnings
- bootstrap confidence intervals if simple

The summarizer must not rely on markdown summaries as ground truth.

## Exit Criteria

Phase 1 is complete only when:

- enriched eval file has 77 non-empty contributions,
- robust audit files are generated,
- known oracle and metadata warnings appear in the audit,
- resume accounting is fixed or safely wrapped,
- `Plan/RUN_STATE.md` points to Phase 2 only after the audit passes.
