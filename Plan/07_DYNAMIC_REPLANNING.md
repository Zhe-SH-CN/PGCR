# Dynamic Replanning Protocol

## Why Dynamic Planning Is Required

The current project idea is plausible, not proven. A CCF-C paper does not truly exist after two chat turns. It becomes a paper only after:

- baseline is measured,
- failures are understood,
- a small method is implemented,
- the method improves something measurable,
- the result story survives ablations.

Therefore the plan must change when evidence changes.

## What Must Stay Fixed

Keep these stable unless the user explicitly changes the project goal:

- Venue target: ICTAI 2026.
- Project type: lightweight Sci-Reasoning Hit@10 improvement.
- Core metric: Hit@10.
- Core model: MiMo.
- Scope: no fine-tuning by default.
- Time budget: finish before 2026-06-30.
- Research integrity: no fabricated numbers or citations.

## What Can Change

Allowed changes:

- prompt wording
- pattern list
- candidate count
- reranking rubric
- diversity penalty
- judge prompt
- fallback eval subset
- table/figure design
- paper framing

Allowed only after evidence:

- adding retrieval
- adding embeddings
- adding another judge
- using a second model for robustness
- building a small graph representation

Not allowed without explicit user approval:

- fine-tuning
- large-scale KG construction
- system-level server changes
- buying paid APIs beyond existing intended usage
- changing the target venue

## Replanning Trigger Conditions

Update the plan when any of these happen:

- baseline cannot run
- eval set cannot be reconstructed
- MiMo output parsing fails on more than 20% of cases
- judge is unstable or too lenient
- PGCR underperforms baseline
- token cost or runtime is too high
- a simple ablation reveals the actual useful component is different from the assumed one

## Replanning Procedure

When triggered, create or update `Plan/RUN_STATE.md` with:

```markdown
## Replan Event YYYY-MM-DD HH:MM

Trigger:

Evidence:

Affected phase:

Options considered:

Decision:

Rollback condition:

Next action:
```

Then make the smallest change that addresses the measured problem.

## Examples

### If vanilla MiMo is already strong

Problem:

- Baseline Hit@10 is near or above 50%.

Response:

- Do not panic.
- PGCR can still win by using candidate expansion.
- Add candidate-count ablation and emphasize efficiency/selection.

### If PGCR does not improve

Problem:

- Pattern prompts generate diverse but low-quality ideas.

Response:

- Check whether reranker is selecting generic ideas.
- Try top-3 patterns only.
- Try hard-case-only PGCR: apply PGCR only to targets where vanilla confidence is low.

### If judge is too permissive

Problem:

- Judge marks vague ideas as matches.

Response:

- Add stricter judge prompt.
- Require same problem and similar methodological approach.
- Run a small human-readable audit sample.

### If data is incomplete

Problem:

- Missing exact 77 NeurIPS Oral target set.

Response:

- Freeze a documented local subset.
- State exact target count and selection criteria in paper.
- Avoid comparing directly to published Gemini 49.35% unless using the same set.

## Paper Framing Pivots

Preferred framing:

- PGCR improves Hit@10.

Fallback framing:

- Candidate expansion and diversity improve scientific ideation.

Second fallback:

- An empirical study of MiMo scientific ideation on Sci-Reasoning.

Do not use a framing unsupported by results.

