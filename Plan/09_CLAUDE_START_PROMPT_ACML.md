# Claude Code Start Prompt for ACML Full Pipeline

Use this prompt for a fresh Claude Code session in a new lab-server folder.

```text
Read CLAUDE.md first.

Then read these files in order:
1. results/acml_readiness_audit.md
2. Plan/00_BIG_PLAN.md
3. Plan/10_PLAN_AUDIT_ACML_READINESS.md
4. research/current_acml_idea_2026-06-12.md
5. research/deep_research_sci_reasoning_2026-06-12.md
6. Plan/RUN_STATE.md
7. Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md
8. Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md
9. The active phase plan named in Plan/RUN_STATE.md

Plan/06 is an operating guide to read before starting. It is not a sixth stage that comes after five manual stages.

At startup, check whether Superpowers and planning-with-files skills are installed. If installed, use Superpowers for execute-plan, systematic debugging, and code review; use planning-with-files for read-before-decide and persistent file-based planning. If either skill is missing, record the missing skill in Plan/RUN_STATE.md and continue with CLAUDE.md and the local Plan/ workflow rather than pretending it was used.

We are targeting ACML 2026 Conference Track. Use the official ACML/JMLR LaTeX submission template from ACML_camera_ready/. Do not continue paper/main.tex as the submission source.

Current working method is BCS: Budgeted Candidate Search. PGCR is current negative-ablation evidence, not the main method. Do not claim PGCR improves performance unless new corrected experiments prove it.

External research found Graph2Idea, FlowPIE, Nova, LDC, SCI-IDEA, RQ-Bench, SciAidanBench, and future-aligned proposal prediction. Do not claim that larger candidate pools alone are novel. The updated lightweight variant is SE-BCS: structured predecessor extraction plus crossover/mutation candidate expansion plus anti-mirage selection diagnostics, all target-hidden and fixed to 10 final ideas.

GitHub remote for this project:
git@github.com:Zhe-SH-CN/PGCR.git

If this lab-server folder is not already a Git repository, initialize Git inside this folder only and set that remote as origin. At every phase or major experiment boundary, after exit criteria pass, update Plan/RUN_STATE.md, run a secret/path sanity scan, commit, and push to origin. Do not commit or push secrets, .env, API keys, private keys, .venv, .uv-cache, or generated caches.

Start with the active phase only. At startup this should be:
Plan/01_PROTOCOL_AND_DATA_REPAIR.md

Hard constraints:
- Use uv with a project-local .venv. After setup, run project scripts with .venv/bin/python.
- Phase 2 main experiments use mimo-v2.5-pro. Phase 3 replication/robustness should use mimo-v2.5 as well, preferably on all 77 targets if time and token budget permit.
- If Phase 2 plain BCS-50 is clean, run SE-BCS-50 or record why it was skipped.
- During experiments, continuously reflect on weaknesses and missing evidence using Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md. At every major part boundary, compare results against research/current_acml_idea_2026-06-12.md and research/deep_research_sci_reasoning_2026-06-12.md, then update Plan/RUN_STATE.md with trigger, evidence, decision, insufficiency/risk, rollback condition, and next action.
- After every completed phase or major part, commit and push to git@github.com:Zhe-SH-CN/PGCR.git after checking that no secrets or private paths are staged.
- Do not report the 58.4% oracle combined result as a fair main method.
- Generation, scoring, selection, and budget allocation must not see target title or target contribution.
- Judge prompts may see target title and contribution only after final ideas are selected.
- Fill missing target contributions before ACML-grade rejudging.
- Every MiMo API call must sleep at least 0.5 seconds.
- Do not parallelize MiMo calls unless the user explicitly approves.
- Do not use sudo, system package installs, destructive commands outside generated caches, or files outside this project directory.
- Do not hardcode API keys or print secrets.
- If optional data is missing, create the smallest documented fallback and continue.

Phase 1 deliverables:
0. Project-local uv environment created and documented.
1. data/scireasoning/eval_neurips_2025_oral_enriched.jsonl with 77 non-empty contributions and contribution_source fields.
2. data/scireasoning/enrichment_report.md.
3. Fixed or wrapped resume accounting for baseline/evaluation scripts.
4. scripts/12_acml_results_audit.py.
5. results/acml_results_audit.json.
6. results/acml_results_audit.md.
7. Updated Plan/RUN_STATE.md with exact completed work, output paths, blockers, and next recommended phase.

Before any long MiMo run, run a 1-3 target smoke test. Phase 1 should prefer no API calls unless needed.
```
