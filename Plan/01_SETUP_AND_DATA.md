# Phase 1 Plan: Setup and Data

## Objective

Prepare the lab-server project so Claude Code can run the Sci-Reasoning Hit@10 experiments without repeatedly rediscovering paths, formats, or API assumptions.

## Assumptions

- The lab server has Python 3.10+.
- MiMo provides an OpenAI-compatible chat/completions endpoint.
- Network access to MiMo is allowed.
- Network access to HuggingFace/OpenReview/arXiv is allowed or datasets are copied manually.
- This phase does not run full experiments.

## Inputs

Required:

- `Sci-Reasoning/` repository.
- `2601.04577v1.pdf` or `Sci-Reasoning/paper.pdf`.
- `Plan/` directory.
- `CLAUDE.md`.
- MiMo API base URL and API key.

Recommended:

- HuggingFace `AmberLJC/Sci-Reasoning` data.
- Local `Sci-Reasoning/prior_work_extraction/results/organized/`.
- Local `wiki/` summaries.
- Optional local PDFs for retrieval.

## Important Ground Truth Rule

Ground truth priority:

1. Sci-Reasoning paper.
2. Sci-Reasoning repository code/results.
3. HuggingFace dataset files.
4. Official ICTAI pages.
5. Local wiki/raw papers.

## Tasks

### 1. Create environment inventory

Record:

- Python version.
- Available package manager: `uv`, `pip`, `conda`, or system Python.
- Disk space.
- Whether network access works.
- Whether MiMo endpoint works.

Output:

- `results/setup/environment_report.md`

### 2. Create MiMo client

Create a minimal reusable client:

- `scripts/mimo_client.py`

Requirements:

- Read credentials from environment variables, not hardcoded files.
- Support `XIAOMI_MIMO_BASE_URL`.
- Support `XIAOMI_MIMO_API_KEY`.
- Support model names such as `mimo-v2.5` and `mimo-v2.5-pro`.
- Default to `sleep_seconds=0.5` between MiMo API calls.
- Retry and back off on HTTP 429/rate-limit errors.
- Avoid parallel MiMo calls by default because Claude Code uses the same MiMo account.
- Log token usage if returned by API.
- Retry transient failures.
- Never print API keys.

Smoke test script:

- `scripts/00_test_mimo_connection.py`

Success criterion:

- One call returns a valid response and writes `results/setup/mimo_smoke_test.json`.

### 3. Build data manifest

Create:

- `scripts/01_prepare_scireasoning_data.py`

Purpose:

- Locate available local Sci-Reasoning data.
- Normalize each paper into a common schema.
- Identify the evaluation set.

Preferred eval set:

- NeurIPS 2025 Oral targets from Sci-Reasoning prior-work extraction.

Fallback eval sets, in order:

1. All available NeurIPS 2025 Oral/Spotlight if Oral labels are incomplete.
2. A fixed stratified 77-paper subset from NeurIPS 2025.
3. A documented 30-paper pilot subset for method debugging only.

Common target schema:

```json
{
  "target_id": "string",
  "title": "string",
  "venue": "NeurIPS",
  "year": 2025,
  "presentation_type": "oral",
  "abstract": "string_or_empty",
  "contribution": "string_or_empty",
  "predecessors": [
    {
      "title": "string",
      "role": "string_or_empty",
      "relationship_sentence": "string_or_empty",
      "synthesis_narrative": "string_or_empty"
    }
  ],
  "source_path": "string"
}
```

Outputs:

- `data/scireasoning/manifest.json`
- `data/scireasoning/eval_neurips_2025_oral.jsonl`
- `data/scireasoning/debug_sample_3.jsonl`

### 4. Define retrieval policy

Keep retrieval simple.

Preferred input context per predecessor:

1. predecessor title
2. role
3. relationship sentence
4. target-level synthesis narrative
5. abstract/full text only if readily available

Do not spend days building a full PDF retrieval system unless baseline quality is blocked by missing context.

### 5. Verify one formatted prompt

Create:

- `scripts/02_render_prompts.py`

Output:

- `results/setup/sample_baseline_prompt.md`
- `results/setup/sample_pattern_prompt.md`

The sample prompt must be readable and not exceed a practical model context window.

## Verification

Phase 1 is complete only when:

- MiMo smoke test passed.
- Data manifest exists.
- Eval set exists or fallback is documented.
- One target can be converted into baseline and pattern prompts.
- `Plan/RUN_STATE.md` is updated.

## Failure Handling

If HuggingFace download fails:

- Use local `Sci-Reasoning/prior_work_extraction/results/organized`.
- Document missing columns.
- Continue with a fixed local subset.

If MiMo fails:

- Test base URL and model list.
- Confirm environment variables.
- Do not rewrite the method before the API works.

If eval labels are inconsistent:

- Freeze a documented subset and proceed.
- The paper should state the exact subset used.
