@AGENTS.md

# CLAUDE.md

Project: ACML 2026 Sci-Reasoning Hit@10 automation.

## Project Owner and Venue Context

The project owner is a Shanghai Jiao Tong University CS graduate student with two NeurIPS papers. The target is a lightweight, defensible ACML 2026 submission, not a large multi-month research program.

ACML 2026 Conference Track constraints:

- Submission deadline: 2026-06-26, 23:59 AoE.
- Format: ACML/JMLR style using the official ACML LaTeX submission template/style files in `ACML_camera_ready/`.
- Full paper length: up to 16 pages including references and appendix.
- Review: double-blind.
- Reviewer nomination is required by ACML unless the PCs approve an exception.
- Practical objective: produce a reproducible experiment package and paper draft that can be submitted if results support the claim.

Do not include the project owner's identity, SJTU affiliation, or publication history in the anonymous submission draft.

## Mission

Build a lightweight, reproducible ACML submission around MiMo-based scientific idea generation on Sci-Reasoning.

The old PGCR-positive claim is rejected by current evidence. The current working method is BCS: Budgeted Candidate Search. PGCR is a negative ablation.

Read `results/acml_readiness_audit.md`, `Plan/00_BIG_PLAN.md`, `Plan/10_PLAN_AUDIT_ACML_READINESS.md`, `Plan/RUN_STATE.md`, `Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md`, `Plan/07_DYNAMIC_REPLANNING_AND_SUBMISSION_GATES.md`, and the active phase plan named in `Plan/RUN_STATE.md` before doing new work.

`Plan/06_AUTOMATION_AND_SERVER_OPERATIONS.md` is an operating guide to read before Phase 1. It is not a later stage that delays Claude Code involvement.

## Repository Handoff

GitHub remote:

```bash
git@github.com:Zhe-SH-CN/PGCR.git
```

If the lab-server project folder is not already a Git repository, initialize Git inside that project folder only and set this remote as `origin`. Do not commit or push API keys, `.env`, `.venv`, `.uv-cache`, logs containing secrets, private paths, or generated caches.

Commit at phase boundaries after the phase exit criteria pass. Push only reproducible code, plans, anonymized paper files, and result artifacts that do not contain secrets.

## Ground Truth Priority

Use sources in this order:

1. Sci-Reasoning paper PDFs.
2. Sci-Reasoning repository code and result JSON files.
3. HuggingFace `AmberLJC/Sci-Reasoning` files.
4. Official ACML pages.
5. Local `wiki/` and `raw/` paper summaries.

## Research Integrity Rules

- Do not fabricate results.
- Do not fabricate citations, DOI values, arXiv IDs, author names, venues, or page numbers.
- Every number used in the paper must be traceable to a result file.
- Generation and reranking prompts must not see the target title or target contribution.
- BCS generation, scoring, selection, and budget allocation must not see target title or target contribution.
- Judge prompts may see the target title and contribution only after ideas are generated.
- Do not present the 58.4% oracle combined result as a fair main method.
- Do not claim PGCR improves performance unless new corrected experiments prove it.
- If a result is weak, report it honestly and pivot the framing instead of inflating claims.

## Scope Rules

- Keep changes small and task-driven.
- Prefer new files under `scripts/`, `data/`, `results/`, `logs/`, and `paper/`.
- Do not refactor the original `Sci-Reasoning/` repository unless a small compatibility wrapper is impossible.
- Do not implement fine-tuning unless the user explicitly approves it.
- Do not build a full graph database unless a phase plan is updated with evidence and user approval.

## Automation Rules

- Before a long run, run a 1-3 item smoke test.
- At startup, check whether Superpowers and planning-with-files skills are installed. Use them if available; continue with the local `Plan/` workflow if they are missing.
- Do not stop for ordinary missing optional data, minor format ambiguity, or recoverable API failures; choose the smallest documented fallback, log it, and continue.
- Ask the user only for secrets, system-level changes, access outside the project directory, destructive deletion beyond generated caches, parallel MiMo execution, fine-tuning, full KG construction, or a major research-scope change.
- Every long-running script must support `--resume`.
- Checkpoint after every target paper.
- Store raw model outputs, parsed JSON, judge outputs, prompt template versions, timestamps, and failures.
- Update `Plan/RUN_STATE.md` at the end of each session.
- If blocked, write the blocker and the smallest next step in `Plan/RUN_STATE.md`.

## Server Safety Rules

- Do not use `sudo`.
- Do not modify system directories.
- Do not read or write files outside this project directory.
- Do not read `.env`, `.ssh`, private keys, credentials, or other users' files.
- Do not hardcode API keys in code.
- Use environment variables for secrets.
- Do not run destructive filesystem commands except for clearly scoped generated caches inside this project.
- Do not install system packages.
- Use `uv` with a project-local `.venv` for Python dependencies.
- Prefer a project-local cache, for example `UV_CACHE_DIR=.uv-cache`.
- After environment setup, run project scripts with `.venv/bin/python`, not the system Python.

Preferred environment setup:

```bash
UV_CACHE_DIR=.uv-cache uv venv .venv
UV_CACHE_DIR=.uv-cache uv pip install -r requirements.txt
```

If `requirements.txt` does not exist, create the smallest dependency file needed for the current phase before installing.

## MiMo Rules

Expected environment variables:

```bash
XIAOMI_MIMO_BASE_URL
XIAOMI_MIMO_API_KEY
MIMO_MODEL
```

Claude Code and the experiments use the same MiMo account. To reduce 429 errors that could also interrupt the main Claude Code session:

- Every experiment script must default to at least `--sleep-seconds 0.5`.
- Sleep at least 0.5 seconds between MiMo API calls, including generation, reranking, and judging calls.
- On HTTP 429 or equivalent rate-limit errors, back off and resume from checkpoints instead of restarting completed work.
- Do not parallelize MiMo calls unless the user explicitly approves it after seeing stable single-worker runs.

Do not print API keys. If the API fails, test a minimal request before changing experiment logic.

Model roles:

- `mimo-v2.5-pro` is the primary model for Phase 2 Direct-10 rejudge and full-set BCS-50/BCS-100 experiments.
- `mimo-v2.5` is the planned replication model in Phase 3.
- ACML-grade evidence should include both models if time and token budget permit; if `mimo-v2.5` is reduced to a subset, mark that result as robustness evidence rather than the main claim.

## Planning Rules

The plan is dynamic but bounded.

When changing the plan, update `Plan/RUN_STATE.md` with:

- trigger
- evidence
- decision
- rollback condition
- next action

Do not jump phases. Complete the current phase exit criteria before moving on.

## Paper Rules

- Use ACML/JMLR anonymous conference format.
- Start from the official ACML LaTeX template `ACML_camera_ready/acml26_submission_template.tex`; do not continue any historical non-ACML draft as the submission source.
- Remove author-identifying paths and institution text from the submission version.
- Keep all paper claims tied to experiment outputs.
- Leave AI-use disclosure as a final author decision, but do not forget venue policy constraints.
