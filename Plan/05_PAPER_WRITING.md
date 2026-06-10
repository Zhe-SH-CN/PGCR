# Phase 5 Plan: ICTAI Paper Writing

## Objective

Generate an anonymous IEEE-style ICTAI 2026 paper draft from completed experiments.

The paper should be honest, compact, and result-driven. Do not write a grand automated-science manifesto.

## Working Title

Preferred:

**Pattern-Guided Candidate Reranking for Scientific Idea Generation**

Alternatives:

- Pattern-Guided Scientific Ideation with Large Language Models
- Improving Research Idea Generation via Innovation Pattern-Guided Reranking

## Paper Claim

Use the strongest claim supported by evidence:

Strong:

> PGCR improves Hit@10 over vanilla MiMo on the Sci-Reasoning NeurIPS 2025 Oral evaluation set.

Moderate:

> Pattern-guided candidate expansion improves candidate diversity and yields higher Hit@10 after reranking.

Weak:

> A controlled study identifies when pattern-guided prompting helps and fails for LLM-based scientific ideation.

## Paper Structure

### Abstract

Include:

- problem: scientific idea generation from predecessors
- limitation: direct LLM generation produces few and often generic candidates
- method: pattern-guided candidate expansion and reranking
- evaluation: Sci-Reasoning Hit@10 protocol
- result: exact measured improvement

Do not include:

- unsupported "first" claims
- vague "revolutionize science" language

### 1. Introduction

Core argument:

1. LLMs can generate research ideas, but direct generation is narrow.
2. Sci-Reasoning identifies recurring innovation patterns.
3. These patterns can be used as generation controls.
4. Wide candidate generation plus reranking is a practical way to improve Hit@10.

End with contributions:

- pattern-guided candidate expansion method
- pattern-aware reranker with diversity selection
- empirical evaluation on Sci-Reasoning Hit@10
- case analysis of successful and failed scientific ideation

### 2. Related Work

Keep concise:

- Sci-Reasoning
- automated research / AI auto-research
- LLM scientific idea generation
- LLM-as-judge evaluation
- knowledge graph / pattern-guided reasoning if needed

Use `awesome-ai-auto-research/README.md` only to find papers, not as primary evidence.

### 3. Task and Baseline

Define:

- input
- output
- Hit@10
- judge
- dataset split
- baseline MiMo prompt

Make leakage prevention explicit:

- generation and reranking do not see target title or target contribution.
- judge sees target title/contribution only after ideas are produced.

### 4. Method

Describe PGCR:

1. pattern-conditioned generation
2. candidate normalization
3. pattern-aware scoring
4. diversity-aware Top-10 selection

Include a small algorithm block if space permits.

### 5. Experiments

Subsections:

- Dataset and protocol
- Main results
- Ablations
- Cost/runtime
- Case studies

Tables:

- Main results table
- Ablation table
- Optional cost/runtime table

### 6. Analysis

Discuss:

- when pattern prompts help
- why reranking helps
- failure cases
- judge sensitivity
- limitations of predecessor-only prediction

### 7. Conclusion

Short and concrete.

## Figures

Minimum figure:

- Pipeline diagram: predecessors -> pattern generation -> candidates -> reranker -> Top-10 -> judge.

Optional:

- Bar chart of Hit@10.
- Candidate count vs Hit@10 curve.

## Evidence Rules

Every number in the paper must point to:

- `results/main_results.json`
- `results/ablation_results.json`
- `results/baseline_mimo.json`
- `results/pgcr_full.json`

Every citation must be real. If a BibTeX entry cannot be verified, remove it.

Do not create fake:

- DOI
- proceedings venue
- arXiv ID
- author list
- page numbers
- acceptance status

## IEEE and Double-Blind Checklist

Before submission:

- Remove author names and affiliations.
- Avoid "our lab", "Shanghai Jiao Tong University", and self-identifying paths.
- Remove absolute local paths from paper.
- Use IEEE conference template.
- Keep full paper <= 8 pages excluding references only if ICTAI rules allow; verify final instructions.
- Ensure figures are readable in grayscale.
- Ensure all table numbers match result files.

## AI Use Statement

IEEE requires disclosure of AI-generated content in acknowledgments for IEEE publications. Whether and how to include it is the user's final decision, but the draft should keep a placeholder comment so it is not forgotten:

```tex
% TODO(author): decide whether to include AI-use disclosure per IEEE policy.
```

Do not insert acknowledgments in the anonymous submission unless ICTAI permits it.

## Deliverables

- `paper/main.tex`
- `paper/references.bib`
- `paper/figures/pipeline.pdf` or `.png`
- `paper/tables/main_results.tex`
- `paper/tables/ablations.tex`
- `paper/build.log`
- `paper/submission_checklist.md`
- Updated `Plan/RUN_STATE.md`

