# Run State

This file is the persistent handoff state for Claude Code sessions.

## Current Status

- Target venue: ACML 2026 Conference Track.
- Current phase: **Paper draft COMPLETE — ready for review**.
- Active plan file: `Plan/05_ACML_PAPER_WRITING.md` (paper refinement).
- GitHub remote: `git@github.com:Zhe-SH-CN/PGCR.git`.
- Last completed work: Paper draft "When More Ideas Do Not Help" (8 pages, 2026-06-13).
- Current decision: Paper ready for review and refinement.
- Skills: Superpowers and planning-with-files confirmed installed by user.

## Complete Results (enriched judging)

| Method | Hits | Total | Hit@10 | 95% CI | Status |
|---|---:|---:|---:|---|---|
| Direct-10 (enriched) | 20 | 77 | 26.0% | [16.9, 36.4] | baseline |
| BCS-50 | 16 | 77 | 20.8% | [11.7, 29.9] | negative |
| PGCR (enriched) | 14 | 77 | 18.2% | [10.4, 27.3] | negative |
| Direct-10 (title-only) | 29 | 77 | 37.7% | [27.3, 48.1] | overestimated |

## Key Findings

1. **Enriched judging is dramatically stricter** — 26.0% vs 37.7% title-only (11.7pp drop).
2. **BCS-50 does not beat Direct-10** — 20.8% vs 26.0% (5.2pp worse).
3. **PGCR also fails** — 18.2% under enriched judging.
4. **BCS and Direct find different targets** — only 5/31 hits overlap.
5. **Judge confidence is well-calibrated** — hit mean ~0.868, miss mean ~0.048.

## Paper Status

- Title: "When More Ideas Do Not Help: Failure Modes of Candidate Expansion for Scientific Ideation"
- Format: ACML/JMLR template (`paper/acml_main.tex`)
- Length: 8 pages (under 16-page limit)
- Status: Draft compiles cleanly, needs citation verification and content review

## What's Done

### Phase 1: Protocol and Data Repair
- Enriched eval data: 77/77 non-empty contributions
- Resume accounting fixed in baseline and eval scripts
- ACML results audit script created
- All Phase 1 exit criteria met

### Phase 2: Fair BCS Experiments
- Direct-10 enriched rejudge: 77/77, 20 hits, 26.0%
- BCS-50 generation: 77/77 targets, 50 candidates each
- BCS-50 scoring/selection: 77/77 targets
- BCS-50 evaluation: 77/77, 16 hits, 20.8%
- PGCR enriched rejudge: 77/77, 14 hits, 18.2%

### Paper Draft
- ACML/JMLR format, 8 pages
- Negative-result framing with enriched judging analysis
- Case studies and failure mode analysis
- Detailed appendix with prompts, tokens, CIs

## What's Missing / Could Be Improved

1. **Citation verification** — some references are anonymous placeholders
2. **Judge robustness** — no second judge or cross-model replication
3. **More detailed analysis** — could add more case studies
4. **Reviewer nomination** — need to handle ACML requirement

## Output Paths

### Data
- `data/scireasoning/eval_neurips_2025_oral_enriched.jsonl` (77 records, enriched)
- `data/scireasoning/enrichment_report.md`

### Results
- `results/acml_direct10_rejudge_mimo_v25pro.json` (20 hits, 26.0%)
- `results/bcs50_eval_mimo_v25pro.json` (16 hits, 20.8%)
- `results/bcs50_candidates_mimo_v25pro.jsonl` (77/77 candidates)
- `results/bcs50_selected_mimo_v25pro.jsonl` (77/77 selected)
- `results/pgcr_enriched_eval.json` (14 hits, 18.2%)
- `results/acml_results_audit.json` (complete audit)
- `results/acml_results_audit.md` (readable audit)

### Scripts
- `scripts/12_enrich_eval_data.py` (enrichment)
- `scripts/13_acml_results_audit.py` (audit)
- `scripts/14_direct10_rejudge.py` (rejudge)
- `scripts/15_bcs_evaluate.py` (BCS evaluation)
- `scripts/mimo_client.py` (MiMo API client with timeout)

### Paper
- `paper/acml_main.tex` (main paper, 8 pages)
- `paper/references.bib` (references)

## Next Steps

1. Verify citations are real (especially anonymous ones for related work).
2. Check paper against ACML submission checklist.
3. Add more detailed analysis if needed.
4. Ensure anonymization is complete.
5. Handle reviewer nomination requirement.
6. Prepare submission materials.

## Blockers

- Need to verify citations are real (especially anonymous ones).
- Paper is 8 pages — could add more content if needed.
- Need to check if reviewer nomination requirement is met.

## Required Read Order (for next session)

1. `CLAUDE.md`
2. `results/acml_readiness_audit.md`
3. `Plan/00_BIG_PLAN.md`
4. `Plan/10_PLAN_AUDIT_ACML_READINESS.md`
5. `research/current_acml_idea_2026-06-12.md`
6. `research/deep_research_sci_reasoning_2026-06-12.md`
7. `Plan/RUN_STATE.md` (this file)
8. `Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md`
9. `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md`
10. `Plan/05_ACML_PAPER_WRITING.md`
