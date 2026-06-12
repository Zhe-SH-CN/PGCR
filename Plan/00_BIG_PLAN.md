# Canonical Big Plan: ACML 2026 Sci-Reasoning Project

## Status

This is the canonical plan for the new ACML-focused Claude Code session.

Any earlier non-ACML, PGCR-positive planning is superseded. Do not reconstruct or follow it. Existing PGCR artifacts remain useful only as negative-ablation evidence.

Target venue:

- ACML 2026 Conference Track.
- Deadline: 2026-06-26, 23:59 AoE.
- Format: ACML/JMLR style using the official ACML LaTeX submission template/style files in `ACML_camera_ready/`.
- Limit: 16 pages including references and appendix.
- Review: double-blind.
- Reviewer nomination is required by ACML unless the PCs approve an exception.

Project GitHub remote:

- `git@github.com:Zhe-SH-CN/PGCR.git`

## Research Framing

Working method:

**BCS: Budgeted Candidate Search.**

External-research refinement:

**SE-BCS: Structured-Evolutionary Budgeted Candidate Search** is a lightweight optional/improved variant inspired by Graph2Idea, FlowPIE, Nova, and RQ-Bench. It adds compact predecessor structure extraction, cheap crossover/mutation candidates, and anti-mirage diversity checks, while preserving target-hidden generation and exactly 10 final ideas.

Working title:

**Budgeted Candidate Search for Scientific Idea Generation**

Core claim to test:

> Given predecessor papers only, target-hidden candidate expansion plus quality/diversity selection improves Sci-Reasoning Hit@10 over direct generation under a fair fixed-output protocol, while pattern-conditioned generation is a negative ablation.

Updated novelty boundary after web and NotebookLM research:

> Do not frame the contribution as candidate expansion in general. Related work already explores graph-structured contexts, iterative search, controllable generation, and evolutionary ideation. The ACML contribution must be framed as a controlled Sci-Reasoning Hit@10 study that isolates fixed-output candidate search under leakage constraints, plus lightweight structured/evolutionary ablations.

Current evidence:

| Method | Target set | Hits | Total | Hit@10 | Use in ACML paper |
|---|---:|---:|---:|---:|---|
| MiMo v2.5-pro direct baseline | full | 29 | 77 | 37.7% | valid baseline after rejudge |
| PGCR full | full | 22 | 77 | 28.6% | negative ablation |
| Vanilla expansion | baseline misses only | 16 | 48 | 33.3% | diagnostic only |
| Oracle combined | mixed | 45 | 77 | 58.4% | not a fair method |

Important: the 58.4% oracle combined result must not be reported as the main method.

Model design:

- Phase 2 main experiments use `mimo-v2.5-pro`.
- Phase 3 replication/robustness uses `mimo-v2.5` in addition to `mimo-v2.5-pro`.
- The paper should not imply cross-model robustness unless both model results are actually available.

## Canonical Plan Files

Read these files in order:

1. `CLAUDE.md`
2. `results/acml_readiness_audit.md`
3. `Plan/00_BIG_PLAN.md`
4. `Plan/10_PLAN_AUDIT_ACML_READINESS.md`
5. `research/current_acml_idea_2026-06-12.md`
6. `research/deep_research_sci_reasoning_2026-06-12.md`
7. `Plan/RUN_STATE.md`
8. `Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md`
9. `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md`
10. Current active phase plan from `Plan/RUN_STATE.md`

Claude Code starts at Phase 1. `Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md` and `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md` are cross-phase operating guides to read up front, not late stages that require manual completion of earlier phases.

Execution phases and support documents:

- `Plan/01_PROTOCOL_AND_DATA_REPAIR.md`: repair data, metadata, resume behavior, and summarization.
- `Plan/02_FAIR_BCS_EXPERIMENTS.md`: run fair full-set BCS experiments.
- `Plan/03_REPLICATION_AND_ROBUSTNESS.md`: MiMo v2.5/pro, judge robustness, optional open-source subset.
- `Plan/04_ANALYSIS_TABLES_AND_CASES.md`: statistics, tables, case studies, failure analysis.
- `Plan/05_ACML_PAPER_WRITING.md`: section-by-section plan to a compiled ACML paper.
- `Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md`: Claude Code, server, safety, logging, and context policy.
- `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md`: evidence-driven pivots and submit/stop gates.
- `Plan/08_ACML_EXISTING_EVIDENCE.md`: supplemental map of existing outputs; not the execution plan.
- `Plan/09_CLAUDE_START_PROMPT_ACML.md`: prompt for the next Claude Code session.
- `Plan/10_PLAN_AUDIT_ACML_READINESS.md`: audit of ACML readiness, experiment sufficiency, and automation handoff.

## Non-Negotiable Protocol Rules

- Final Hit@10 output is exactly 10 ideas per target.
- Generation, scoring, selection, and budget allocation must not see target title or target contribution.
- Judge prompts may see target title and contribution only after final ideas are selected.
- Fill target contributions before any ACML-grade rejudging.
- SE-BCS structure extraction, crossover, mutation, and anti-mirage selection must also remain target-hidden.
- Do not tune selection parameters on all 77 targets without labeling the result exploratory.
- Use `uv` with a project-local `.venv`; run project Python scripts through `.venv/bin/python` after setup.
- Every MiMo API call must sleep at least 0.5 seconds.
- Do not parallelize MiMo calls without explicit user approval.
- Every reported number must trace to a JSON/JSONL file.
- Do not fabricate citations, venues, arXiv IDs, DOI values, results, or costs.

## End-to-End Timeline

Current date: 2026-06-11.

- 2026-06-11 to 2026-06-12: Phase 1 protocol and data repair.
- 2026-06-12 to 2026-06-14: Phase 2 full-set BCS-50 and enriched rejudging.
- 2026-06-14 to 2026-06-16: budget curve, BCS-100 if useful, and selection ablations.
- 2026-06-16 to 2026-06-18: Phase 3 replication and judge robustness.
- 2026-06-18 to 2026-06-19: optional local/open-source model subset.
- 2026-06-19 to 2026-06-21: Phase 5 paper draft from ACML template.
- 2026-06-21 to 2026-06-23: internal review, citation verification, anonymization, reviewer nomination check.
- 2026-06-24 to 2026-06-25: final compile, figures, tables, appendix, OpenReview metadata.
- 2026-06-26: submit before AoE deadline.

## Submission Bar

Submit to ACML if:

- Full-set BCS beats the direct baseline by at least 5 percentage points, or
- improvement is smaller but consistent across two models or judges and supported by strong budget/selection analysis.

Weak submit if:

- BCS helps only under an oracle-free difficulty allocator and the analysis is clean.

Do not submit an improvement paper if:

- full-set BCS does not beat direct baseline,
- enriched judging reverses the apparent expansion gain,
- judge agreement is poor and unexplained,
- the ACML paper cannot compile cleanly by 2026-06-24.
