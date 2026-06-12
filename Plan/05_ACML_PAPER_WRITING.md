# Phase 5: ACML Paper Writing

## Objective

Produce a submission-ready ACML/JMLR paper from verified experiment outputs.

Do not reuse `paper/main.tex` as the submission source. It is a historical non-ACML draft and should be treated only as notes.

Use the official ACML LaTeX submission template/style files already stored in `ACML_camera_ready/`.

## Output Files

Create:

- `paper/acml_main.tex`
- `paper/acml_references.bib`
- `paper/acml_submission_checklist.md`
- `paper/tables/main_results.tex`
- `paper/tables/selection_ablation.tex`
- `paper/tables/robustness.tex`
- `paper/tables/budget_curve.tex`
- `paper/figures/`

Start from the official ACML template:

- `ACML_camera_ready/acml26_submission_template.tex`
- `ACML_camera_ready/jmlr.cls`

## Paper Claim Selection

Pick one framing after Phase 4:

### Improvement Paper

Use if full-set BCS or SE-BCS beats direct baseline clearly.

Claim:

> Budgeted target-hidden candidate search improves scientific idea generation over direct generation on Sci-Reasoning.

### Budget/Analysis Paper

Use if BCS improvement is modest but consistent and analytically useful.

Claim:

> Candidate budget, selection, and judge design strongly shape measured scientific ideation performance.

### Negative-Result Paper

Use only if the analysis is strong.

Claim:

> Pattern-conditioned and expanded candidate search reveal failure modes in current Sci-Reasoning evaluation.

Do not write an improvement claim if full-set BCS does not improve.

## Section-by-Section Plan

### Title

Candidate titles:

- Budgeted Candidate Search for Scientific Idea Generation
- Does More Sampling Improve Scientific Ideation? A Sci-Reasoning Study
- Target-Hidden Candidate Expansion for LLM Scientific Ideation

Choose a title that matches the actual result.

### Abstract

Include exactly:

- task: generating scientific ideas from predecessor papers,
- method: target-hidden candidate expansion and selection,
- if available, lightweight structured/evolutionary candidate search,
- benchmark: Sci-Reasoning NeurIPS 2025 Oral subset,
- main result: Direct-10 vs BCS full-set result,
- negative finding: PGCR/pattern conditioning underperforms if still true,
- robustness evidence: second model or second judge if available,
- limitation: single benchmark and LLM judge dependence.

No oracle combined result in the abstract.

### 1. Introduction

Goals:

- motivate scientific ideation as a search problem,
- explain why direct 10-sample prompting can miss target-like ideas,
- state that output budget remains fixed at 10 ideas,
- define the fairness constraint: selection cannot see target,
- preview PGCR as a negative ablation, not the main claim.

Contributions:

1. BCS, a simple target-hidden candidate expansion and selection protocol.
2. A fair full-set evaluation on Sci-Reasoning with enriched target contributions.
3. A budget/selection analysis showing when more candidates help or fail.
4. A negative result on pattern-conditioned generation.

If SE-BCS wins, add:

5. A lightweight structured/evolutionary variant that borrows graph-style context compression and crossover/mutation without full KG, RL, or heavy iterative search infrastructure.

### 2. Background and Task

Cover:

- Sci-Reasoning dataset and Hit@10 protocol,
- predecessor papers, synthesis narratives, target papers,
- Direct-10 baseline,
- why title/contribution enrichment matters,
- double-blind and target-hidden protocol.

Avoid:

- claiming Sci-Reasoning is our dataset,
- claiming SOTA unless directly supported.

### 3. Method: Budgeted Candidate Search

Define:

- candidate budget `B`,
- generator `G`,
- target-hidden scorer `S`,
- diversity selector `D`,
- final selected set of 10 ideas.

Subsections:

- Candidate generation.
- Structured predecessor compression, if SE-BCS is used.
- Crossover and mutation candidate generation, if SE-BCS is used.
- Target-hidden scoring.
- Diversity-aware selection.
- Anti-mirage selection diagnostics.
- Baseline-preserving mixture, if used.
- Computational cost.

Make explicit:

- target title and contribution are never used before final judging,
- final output size is always 10,
- candidate expansion increases search budget but not evaluation output budget.

### 4. Experimental Setup

Include:

- dataset: 77 NeurIPS 2025 Oral targets,
- enriched contribution construction,
- models: MiMo v2.5-pro, MiMo v2.5 if run, local model if run,
- judge protocol,
- prompt versions,
- temperature and candidate budgets,
- sleep/rate-limit protocol,
- metrics and statistical tests.

### 5. Main Results

Use tables generated in Phase 4.

Required discussion:

- full-set Direct-10 vs BCS-50,
- SE-BCS-50 if available,
- BCS-100 budget curve if available,
- PGCR negative ablation,
- selection ablations,
- token-cost tradeoff.

Do not include 58.4% as a main method. It may appear only as "oracle diagnostic upper bound" if clearly labeled and useful.

### 6. Analysis

Subsections:

- When BCS helps.
- When BCS hurts.
- Selection failures.
- Pattern-conditioning failures.
- Difficulty by innovation pattern and predecessor count.
- Judge robustness.

Every case study must cite a result file path in comments or appendix notes.

### 7. Related Work

Minimum topics:

- Sci-Reasoning and scientific ideation benchmarks,
- LLM-based research idea generation,
- candidate generation/reranking/search,
- graph-structured scientific ideation such as Graph2Idea,
- evolutionary or iterative ideation such as FlowPIE and Nova,
- controllable ideation such as LDC,
- evaluation-bias work such as RQ-Bench and human expert studies,
- self-consistency and sampling budgets,
- LLM-as-judge evaluation and its limitations,
- AI for science / automated research agents if space permits.

Use only real, verified citations. No placeholder BibTeX.

### 8. Limitations

Must mention:

- single benchmark and only NeurIPS 2025 Oral subset,
- LLM judge dependence,
- target contribution enrichment may introduce noise,
- MiMo-specific results may not generalize,
- candidate expansion increases token cost,
- generated ideas are not validated by actually writing or running downstream research.

### 9. Conclusion

Keep it concise:

- BCS is a simple search-budget intervention,
- result depends on fair full-set evidence,
- pattern prompting can hurt recall,
- future work should study better target-hidden selectors and human evaluation.

### Appendix

Include:

- prompts,
- enriched data construction details,
- additional result tables,
- case studies,
- model and run metadata,
- failure examples.

## Citation Rules

- No fake references.
- No placeholder entries.
- Verify every BibTeX entry by title, author, venue, and year.
- Prefer existing local papers and official sources when available.
- If a citation is uncertain, remove it rather than guessing.

## Compile Gate

Before final submission:

- compile `paper/acml_main.tex`,
- inspect PDF manually or with logs,
- fix missing citations and overfull boxes,
- confirm page limit,
- confirm anonymous author block,
- confirm no local path leaks.

## Exit Criteria

Phase 5 is complete when:

- ACML PDF compiles cleanly,
- all numbers match `results/acml_results_audit.json`,
- references are real,
- checklist passes,
- `Plan/RUN_STATE.md` records final submit/stop decision.
