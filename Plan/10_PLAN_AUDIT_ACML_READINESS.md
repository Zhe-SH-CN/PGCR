# Plan Audit: ACML Readiness and Automation

Date: 2026-06-11

## Direct Answers

Claude Code should not wait until `Plan/06` to start. It starts at Phase 1 with `Plan/01_PROTOCOL_AND_DATA_REPAIR.md`. `Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md` is a pre-flight operating guide that Claude Code reads before Phase 1.

The Python environment rule is now explicit: use `uv`, create a project-local `.venv`, prefer `UV_CACHE_DIR=.uv-cache`, and run project scripts with `.venv/bin/python` after setup.

The current BCS idea is not yet proven to be ACML-level. It is a plausible ACML submission direction only if the full-set, target-hidden, oracle-free experiments show consistent improvement over the direct baseline and the paper presents clean analysis. Current evidence is insufficient because the only strong-looking 58.4% number is oracle-style and cannot be used as the main method.

The current experiment quantity is not enough for ACML. Existing results are useful for diagnosis, but the submission needs the planned full-set BCS runs, enriched judging, ablations, robustness checks, and statistical analysis.

The plan is structurally complete after this audit: it covers environment setup, data repair, full experiments, robustness, analysis, ACML paper writing, automation rules, dynamic replanning, and submission gates.

## Requirement-to-Evidence Map

| Requirement | Current plan evidence |
|---|---|
| Use a `uv` virtual environment | `CLAUDE.md`, `Plan/00_BIG_PLAN.md`, `Plan/01_PROTOCOL_AND_DATA_REPAIR.md`, `Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md`, and `Plan/09_CLAUDE_START_PROMPT_ACML.md` require `uv`, project-local `.venv`, and `.venv/bin/python`. |
| Judge whether the idea is ACML-level | This file states BCS is not yet proven ACML-level; `Plan/00_BIG_PLAN.md` defines the submission bar; `Plan/02_FAIR_BCS_EXPERIMENTS.md` and `Plan/03_REPLICATION_AND_ROBUSTNESS.md` define the evidence needed. |
| Judge whether experiment quantity is enough | This file says current experiments are insufficient; the minimum evidence package below lists missing full-set BCS, budget, ablation, robustness, statistics, and case-study requirements. |
| Include reflection and dynamic adjustment | `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md` defines triggers, pivots, submit/weak-submit/stop gates, and required `Plan/RUN_STATE.md` update fields. |
| Cover the full path from current data to paper | `Plan/01` repairs data, `Plan/02` runs fair experiments, `Plan/03` adds robustness, `Plan/04` generates evidence tables/cases, and `Plan/05` writes and compiles the paper. |
| Use the official ACML LaTeX template | `CLAUDE.md`, `Plan/00`, `Plan/05`, `Plan/09`, and this file require starting from `ACML_camera_ready/acml26_submission_template.tex` and `ACML_camera_ready/jmlr.cls`. |

## Official ACML Check

The plan matches the ACML 2026 Conference Track page checked on 2026-06-11:

- deadline: 2026-06-26, 23:59 AoE,
- conference track length: 16 pages including references and appendix,
- manuscripts must follow the LaTeX submission template and style file,
- submissions must be anonymized for double-blind review,
- reviewer nomination is required unless an exception applies.

Official page: https://www.acml-conf.org/2026/calls/papers/

## Minimum Evidence Package

A credible ACML submission needs at least:

- Direct-10 baseline rejudged on the enriched 77-target file.
- BCS-50 on all 77 targets under target-hidden generation, scoring, selection, and budget allocation.
- One budget curve point such as BCS-100 or a cheaper BCS-25/50/100 curve if tokens allow.
- Selection ablations, for example random selection, quality-only, diversity-only, and quality-plus-diversity.
- PGCR reported only as a negative or failed ablation unless corrected experiments overturn the current result.
- Judge robustness through a second prompt, second judge model, or carefully documented spot audit.
- Model robustness through MiMo v2.5 versus MiMo v2.5-pro if feasible.
- Bootstrap confidence intervals or another simple uncertainty estimate.
- Token/cost table with the required 0.5-second sleep rule documented.
- Case studies and failure analysis tied to exact result files.

Without these, the project may still be a useful token-burning automation experiment, but it should not be represented as ACML-ready.

## Dynamic Adjustment Mechanism

Use `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md` as the governing mechanism.

After every phase, Claude Code must update `Plan/RUN_STATE.md` with:

- trigger,
- evidence,
- decision,
- rollback condition,
- next action.

Allowed pivots include changing the claim strength, turning a positive-method paper into a budget/analysis paper, or stopping before submission if full-set BCS does not beat the direct baseline.

## End-to-End Coverage

The current plan covers the whole path from current data to submission draft:

- Phase 1: repair data, contributions, metadata, resume accounting, and result auditing.
- Phase 2: run fair BCS experiments.
- Phase 3: replication and robustness.
- Phase 4: tables, statistics, figures, case studies, and leakage checks.
- Phase 5: write and compile the ACML paper.
- Operating guide: server safety, MiMo rate limiting, logging, context handoff.
- Dynamic gates: submit, weak-submit, pivot, or stop decisions.

The ACML paper must start from the official ACML LaTeX submission template `ACML_camera_ready/acml26_submission_template.tex`. Historical non-ACML drafts, especially `paper/main.tex`, are not the submission source.

## Current Verdict

The plan is now suitable to hand to Claude Code from Phase 1. The research result itself is still conditional: BCS becomes an ACML-level claim only if the full-set fair experiments beat the enriched direct baseline with enough robustness and analysis.
