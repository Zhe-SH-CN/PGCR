# Phase 7: Dynamic Replanning and Submission Gates

## Objective

Adapt based on evidence without drifting into an unfinishable project.

The plan is dynamic, but the venue deadline and protocol integrity are fixed.

## Fixed Constraints

Do not change without explicit user approval:

- target venue: ACML 2026 Conference Track,
- final output: ACML/JMLR paper,
- target-hidden generation/scoring/selection,
- exactly 10 final ideas per target,
- no oracle combined result as a main method,
- no system-level server changes,
- no parallel MiMo calls,
- no fine-tuning or full KG construction.

## What Can Change Freely

Claude Code may change:

- prompts,
- selection weights,
- diversity thresholds,
- candidate budgets among 20/50/100,
- summarizer implementation,
- table formatting,
- case study choices,
- paper framing among improvement, budget-analysis, and negative-result paper.

Every change must be recorded in `Plan/RUN_STATE.md` with:

- trigger,
- evidence,
- decision,
- insufficiency or risk,
- rollback condition,
- next action.

## Reflection Cadence

Claude Code must reflect before continuing at every major part boundary:

- Phase 1 protocol/data repair,
- Direct-10 enriched rejudge,
- BCS-50 full run,
- SE-BCS-50 full run or skip decision,
- Phase 3 robustness,
- Phase 4 tables/analysis,
- Phase 5 paper draft/compile.

At each reflection point:

1. Compare current evidence against `research/current_acml_idea_2026-06-12.md`.
2. Compare related-work risk against `research/deep_research_sci_reasoning_2026-06-12.md`.
3. Record what is weak, missing, contradictory, or likely not ACML-level.
4. Decide whether to continue, run SE-BCS, weaken the claim, pivot, or stop.
5. Update `Plan/RUN_STATE.md`.
6. After exit criteria pass, commit and push to `origin`.

This reflection must happen during experimentation, not only after all experiments finish.

## Replanning Triggers

Replan if:

- enriched contribution matching fails for more than 5 targets,
- BCS-50 does not beat Direct-10,
- BCS improves but loses many Direct-10 hits,
- judge agreement is poor,
- MiMo 429 errors block long runs,
- ACML paper cannot compile,
- page limit cannot be met,
- reviewer nomination requirement is unresolved by 2026-06-23.

## Submit / Pivot / Stop

### Submit

Submit if:

- full-set BCS improves direct baseline by at least 5 percentage points, or
- smaller improvement is replicated across model/judge and has strong budget-selection analysis,
- paper compiles cleanly,
- citations are verified,
- anonymization passes.

### Weak Submit

Weak submit if:

- BCS improvement is small but consistent,
- negative PGCR result and selection analysis are interesting,
- limitations are honest,
- time is enough to produce a clean ACML paper.

### Pivot

Pivot to an empirical analysis paper if:

- BCS does not improve,
- but experiments reveal robust failure modes about candidate expansion, pattern conditioning, or LLM judging.

Possible pivot title:

**When More Ideas Do Not Help: Failure Modes of Candidate Expansion for Scientific Ideation**

### Stop

Stop ACML submission if:

- enriched judging invalidates the main result,
- no non-oracle method beats direct baseline,
- judge instability cannot be bounded,
- paper cannot be stabilized by 2026-06-24.

## Final Pre-Submission Gate

Before submission, verify:

- `paper/acml_main.tex` compiles,
- PDF is anonymous,
- reviewer nomination handled,
- no local paths or secrets,
- all tables match `results/acml_results_audit.json`,
- no fake citations,
- no oracle result as main method,
- `paper/acml_submission_checklist.md` is fully checked.
