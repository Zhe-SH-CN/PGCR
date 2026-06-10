# Big Plan: ICTAI 2026 Sci-Reasoning Hit@10 Project

## Purpose

Build a lightweight, automated research project for ICTAI 2026 around Sci-Reasoning idea generation. The project should burn MiMo tokens productively, produce reproducible experiment logs, and generate an IEEE-style paper draft.

The target is not to build a large new benchmark or train a model. The target is to improve a MiMo baseline on the Sci-Reasoning Hit@10 protocol using a small, defensible method:

**PGCR: Pattern-Guided Candidate Expansion and Reranking.**

Project owner context:

- Shanghai Jiao Tong University CS graduate student.
- Two NeurIPS papers.
- Goal: attempt a low-workload ICTAI 2026 / CCF-C submission by 2026-06-30.
- Constraint: the anonymous paper draft must not include author identity, SJTU affiliation, or publication history.

## Core Hypothesis

Sci-Reasoning shows that high-quality AI papers follow recurring innovation patterns. Instead of asking MiMo to generate only 10 ideas directly, we can:

1. Condition MiMo on different innovation patterns.
2. Generate many candidate ideas from the same predecessor set.
3. Rerank candidates using pattern fit, predecessor grounding, specificity, novelty, and diversity.
4. Select the final Top-10 for Hit@10 evaluation.

Expected claim:

> Pattern-guided candidate expansion and reranking improves scientific idea generation under the Sci-Reasoning Hit@10 protocol compared with vanilla MiMo generation.

## Non-Goals

- Do not train or fine-tune a model unless every lightweight method fails.
- Do not build a full-scale academic knowledge graph.
- Do not claim SOTA unless the full evaluation supports it.
- Do not fabricate results, citations, related work, or paper claims.

## Venue Constraints

ICTAI 2026:

- Deadline: 2026-06-30.
- Format: IEEE double-column.
- Length: full paper up to 8 pages.
- Review: double-blind.
- Scope match: LLMs, NLP, knowledge representation/reasoning, knowledge extraction, decision support.
- Official pages:
  - https://ictai.computer.org/2026/
  - https://ictai.computer.org/2026/submissions/

## Research Assets

Local assets:

- `2601.04577v1.pdf`: Sci-Reasoning arXiv PDF.
- `Sci-Reasoning/`: first-author open-source repository.
- `raw/`: related KG and automated-science papers.
- `wiki/`: local summaries and concept notes.
- `awesome-ai-auto-research/`: survey/list repository, useful for related work only.

External assets likely needed on the lab server:

- HuggingFace dataset: https://huggingface.co/datasets/AmberLJC/Sci-Reasoning
- Prior-work extraction files under `prior_work_extraction/`.
- MiMo-compatible OpenAI-style endpoint credentials.
- Optional retrieval APIs: OpenReview, arXiv, Semantic Scholar, Exa or local PDF cache.

MiMo account constraint:

- Claude Code and the experiment pipeline use the same MiMo account.
- Every script that calls MiMo must default to `--sleep-seconds 0.5`.
- Sleep at least 0.5 seconds between generation, reranking, and judging calls.
- Do not run parallel MiMo workers unless the user explicitly approves after stable single-worker runs.
- On 429/rate-limit failures, back off and resume from checkpoints.

## Method Summary

### Baseline

Vanilla MiMo:

- Input: predecessor papers for one target paper.
- Output: exactly 10 research ideas.
- Metric: Hit@10 under a fixed judge.

### Proposed Method: PGCR

For each target paper:

1. Read predecessor titles, roles, relationship sentences, and synthesis narrative.
2. Generate candidates under multiple pattern prompts:
   - Gap-Driven Reframing
   - Cross-Domain Synthesis
   - Representation Shift
   - Data & Evaluation Engineering
   - Formal-Experimental Tightening
   - Gap + Representation
   - Cross-Domain + Representation
   - Gap + Cross-Domain
3. Produce 80-120 candidate ideas.
4. Normalize ideas into JSON.
5. Score each idea with a MiMo reranker:
   - predecessor grounding
   - pattern fit
   - specificity
   - plausibility
   - target-like contribution likelihood
   - redundancy penalty
6. Select Top-10 with diversity-aware reranking.
7. Judge Hit@10 using the same judge protocol across all methods.

## Phases

### Phase 1: Setup and Data

Read: `Plan/01_SETUP_AND_DATA.md`

Goal: create a clean local dataset and runnable MiMo client.

Exit criteria:

- Data manifest exists.
- 77 NeurIPS 2025 Oral evaluation targets are identified or a documented fallback split is defined.
- One MiMo API smoke test succeeds.
- One sample target can be loaded and formatted.

### Phase 2: Baseline Reproduction

Read: `Plan/02_BASELINE_REPRODUCTION.md`

Goal: run vanilla MiMo Hit@10 on the eval set.

Exit criteria:

- Baseline results JSON exists.
- Judge outputs are stored.
- Failure cases are summarized.
- Token/cost/time logs are available.

### Phase 3: PGCR Implementation

Read: `Plan/03_METHOD_PGCR.md`

Goal: implement pattern-guided generation, scoring, and selection.

Exit criteria:

- Candidate generation works for one target.
- Reranker returns valid structured scores.
- Top-10 selector is deterministic given fixed inputs.
- Full method can run on a 3-paper smoke test.

### Phase 4: Experiments and Ablations

Read: `Plan/04_EXPERIMENTS_AND_ABLATIONS.md`

Goal: run full evaluation and enough ablations to support the paper.

Exit criteria:

- Main results table is generated.
- Ablation table is generated.
- At least 3 case studies and 3 failure cases are saved.
- A decision is made: submit, pivot, or stop.

### Phase 5: Paper Draft

Read: `Plan/05_PAPER_WRITING.md`

Goal: produce an 8-page IEEE-style ICTAI paper draft.

Exit criteria:

- `paper/main.tex` compiles.
- All reported numbers are traceable to result files.
- References are real and checked.
- Anonymous submission version is ready.

### Phase 0: Claude Code Operating Manual

Read before Phase 1 and throughout the project: `Plan/06_AUTOMATION_AND_CLAUDE_CODE.md`

Goal: start Claude Code immediately and keep all long runs safe, resumable, and context-efficient.

Exit criteria:

- Claude Code starts from Phase 1, not after Phase 5.
- Claude follows project boundaries.
- Every long run checkpoints.
- `Plan/RUN_STATE.md` is updated at the end of each session.

### Phase 7: Dynamic Replanning

Read when experiments disagree with assumptions: `Plan/07_DYNAMIC_REPLANNING.md`

Goal: adapt the plan based on evidence without drifting into a large, unfinishable project.

Exit criteria:

- Any plan change has a written reason, evidence, and rollback condition.

## Dynamic Planning Policy

Yes, the plan must be dynamic. The current idea is plausible, not guaranteed. A publishable CCF-C project does not appear fully formed after two chat turns; it becomes publishable only if the experiments produce a clean story.

However, dynamic planning must be bounded:

- Change prompts freely.
- Change reranking weights freely.
- Add small ablations freely.
- Do not expand into fine-tuning, full KG construction, or new benchmark creation unless baseline and PGCR both fail and time remains.
- Keep every plan change tied to a measured failure.

The research loop is:

1. Hypothesis.
2. Implementation.
3. Fixed-metric experiment.
4. Error analysis.
5. Small plan adjustment.
6. Repeat.

This follows the useful part of Karpathy-style `autoresearch`: fixed metric, tight scope, comparable experiments, and logs.

## Minimum Submit Story

Submit if all conditions hold:

- PGCR improves vanilla MiMo by a clear margin on the chosen evaluation set.
- The method is simple enough to explain in one page.
- At least one ablation supports the role of pattern-guided generation or reranking.
- Case studies show PGCR finds target-like ideas vanilla MiMo misses.

Do not submit if:

- The baseline is not reproducible.
- The judge is unstable and no mitigation is implemented.
- Results are worse than vanilla MiMo and no honest angle remains.
- The draft contains unchecked numbers or fake citations.

## Recommended Paper Title Options

- Pattern-Guided Candidate Reranking for Scientific Idea Generation
- Pattern-Guided Scientific Ideation with Large Language Models
- Improving Research Idea Generation via Innovation Pattern-Guided Reranking

## Final Deliverables

Expected final directory structure:

```text
data/
  scireasoning/
    manifest.json
    eval_neurips_2025_oral.jsonl
results/
  baseline_mimo.json
  pgcr_full.json
  ablations.json
  case_studies.md
  run_log.md
paper/
  main.tex
  references.bib
  figures/
  tables/
Plan/
  RUN_STATE.md
```
