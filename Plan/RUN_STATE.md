# Run State

This file is the persistent handoff state for Claude Code sessions.

## Current Status

- Current phase: Phase 1 (Setup and Data) — COMPLETE
- Last completed phase: Phase 1
- Active plan file: `Plan/02_BASELINE_REPRODUCTION.md`
- Best known baseline Hit@10: unknown (Phase 2)
- Best known PGCR Hit@10: unknown (Phase 3)
- Current decision: Phase 1 exit criteria met; proceed to Phase 2.

## Phase 1 Results

### What Worked

- **Environment:** Python 3.12.4, uv available, 1.3TB disk free.
- **Virtual environment:** `.venv` created with uv, packages installed (datasets, requests, python-dotenv, openai).
- **MiMo API:** Smoke test passed. Model `mimo-v2.5-pro` responds correctly. Endpoint: `https://token-plan-cn.xiaomimimo.com/v1`.
- **Local data sources:** All three sources available and matched:
  - `ml_paper_acquisition/results/data/2025/oral_spotlight_papers_2025.json` (1,676 papers, 77 NeurIPS 2025 Oral)
  - `prior_work_extraction/results/organized/NeurIPS_2025/` (764 files, all 77 Oral papers matched)
  - `thinking_patterns_llm_analysis/results/classified_papers.json` (3,291 papers, all 77 Oral papers matched)
- **Eval set:** 77 NeurIPS 2025 Oral papers confirmed. 530 total predecessors, avg 6.9 per target.
- **Pattern distribution:** Gap-Driven Reframing (18), Cross-Domain Synthesis (17), Formal-Experimental Tightening (12), Representation Shift (12), plus 9 other patterns.

### What Failed

- **HuggingFace download:** Rate-limited (429) by hf-mirror.com. Used local data fallback per plan. No data quality impact — local sources are complete.

### Paths to Outputs

| Output | Path |
|--------|------|
| Environment report | `results/setup/environment_report.md` |
| MiMo smoke test | `results/setup/mimo_smoke_test.json` |
| MiMo client | `scripts/mimo_client.py` |
| MiMo smoke test script | `scripts/00_test_mimo_connection.py` |
| Data preparation script | `scripts/01_prepare_scireasoning_data.py` |
| Eval set (77 papers) | `data/scireasoning/eval_neurips_2025_oral.jsonl` |
| Debug sample (3 papers) | `data/scireasoning/debug_sample_3.jsonl` |
| Data manifest | `data/scireasoning/manifest.json` |
| Prompt rendering script | `scripts/02_render_prompts.py` |
| Sample baseline prompt | `results/setup/sample_baseline_prompt.md` |
| Sample pattern prompt | `results/setup/sample_pattern_prompt.md` |

### Key Data Facts

- **Eval set:** 77 NeurIPS 2025 Oral papers
- **Predecessors:** 530 total, 6.9 avg per target, 0 missing
- **Patterns:** 13 distinct primary patterns across 77 targets
- **MiMo model:** mimo-v2.5-pro, ~12.6s per call, 266 input / 50 output tokens for smoke test
- **Prompt size:** ~1,300-1,500 tokens per prompt (well within context window)

## Known Constraints

- Target venue is ICTAI 2026 / CCF-C, submission deadline 2026-06-30.
- Project owner is an SJTU CS graduate student with two NeurIPS papers, but author identity must not appear in the anonymous paper draft.
- Prefer new scripts under `scripts/`.
- Keep experiments resumable and logged.
- Do not hardcode API keys.
- Do not modify files outside the project directory.
- Claude Code and experiments share the same MiMo account.
- Every MiMo API call in experiment scripts must sleep at least 0.5 seconds by default.
- Do not parallelize MiMo calls unless the user explicitly approves it.

## Open Questions

- Which judge model will be used for the main result? (MiMo self-judge, or external model?)
- What temperature and max_tokens settings work best for idea generation?
- How stable is the judge across multiple runs?

## Next Action

Read `Plan/02_BASELINE_REPRODUCTION.md` and execute Phase 2: run vanilla MiMo Hit@10 on the 77-paper eval set.
