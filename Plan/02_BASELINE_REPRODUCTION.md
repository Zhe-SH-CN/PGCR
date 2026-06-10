# Phase 2 Plan: Baseline Reproduction

## Objective

Measure vanilla MiMo performance under a fixed Sci-Reasoning-style Hit@10 protocol. This establishes the baseline that PGCR must beat.

## Inputs

- `data/scireasoning/eval_neurips_2025_oral.jsonl`
- `scripts/mimo_client.py`
- Rendered baseline prompt template.
- Fixed judge prompt template.

## Baseline Definition

For each target paper:

1. Give MiMo only predecessor information.
2. Ask for exactly 10 research ideas.
3. Parse ideas into structured JSON.
4. Judge each idea against the target title and contribution.
5. Hit@10 is true if any idea matches.

## Critical Evaluation Rule

Use the same judge settings for all methods. Do not compare methods under different judges.

Preferred judge:

- MiMo judge if the paper is framed as "MiMo-only automated research".

Optional robustness judges:

- GPT/Claude/Gemini only if credentials are available and results are clearly labeled as secondary robustness checks.

## Scripts to Create

### `scripts/03_run_baseline_mimo.py`

Arguments:

- `--input data/scireasoning/eval_neurips_2025_oral.jsonl`
- `--output results/baseline_mimo.json`
- `--model mimo-v2.5-pro`
- `--limit N`
- `--resume`
- `--sleep-seconds 0.5`

Requirements:

- Checkpoint after every paper.
- Skip completed target IDs on resume.
- Default to sleeping at least 0.5 seconds between MiMo calls.
- Back off and resume on 429/rate-limit errors.
- Do not parallelize MiMo calls by default.
- Store raw model output.
- Store parsed ideas.
- Store judge output.
- Store token usage if available.
- Store failures without crashing the whole run.

### `scripts/04_judge_ideas.py`

Can be integrated into baseline script, but keep judge prompt centralized.

Judge output schema:

```json
{
  "target_id": "string",
  "idea_index": 0,
  "match": true,
  "confidence": 0.0,
  "reason": "string"
}
```

### `scripts/05_summarize_results.py`

Summarize:

- total targets
- completed targets
- failed targets
- hits
- Hit@10
- average generated ideas
- average tokens
- top error types

Output:

- `results/baseline_summary.md`

## Prompt Requirements

Baseline generation prompt:

- No target title.
- No target contribution.
- No leaked ground truth.
- Include predecessor titles and relationship information.
- Ask for exactly 10 ideas.
- Ask for JSON to reduce parsing failures.

Judge prompt:

- Include generated idea.
- Include target title and contribution.
- Ask for semantic match.
- Return JSON only.

## Pilot Before Full Run

Run order:

1. `--limit 3` smoke test.
2. Inspect parsed ideas and judge outputs.
3. Fix parsing issues only.
4. `--limit 10` pilot.
5. Full eval set.

## Success Criteria

Phase 2 is complete when:

- `results/baseline_mimo.json` exists.
- `results/baseline_summary.md` exists.
- At least 95% of eval targets complete, or failures are explained.
- At least 5 miss cases are saved for method design.

## Red Flags

Stop and inspect if:

- Hit@10 is implausibly high above 70%.
- Judge says almost everything is a match.
- Target title/contribution appears in generation prompts.
- Parsed ideas are fewer than 8 for many papers.
- Many responses contain generic vague ideas.

## Deliverables

- `results/baseline_mimo.json`
- `results/baseline_summary.md`
- `results/failure_cases_baseline.md`
- Updated `Plan/RUN_STATE.md`
