@AGENTS.md

# CLAUDE.md

Project: ICTAI 2026 Sci-Reasoning Hit@10 automation.

## Project Owner and Venue Context

The project owner is a Shanghai Jiao Tong University CS graduate student with two NeurIPS papers. The target is a lightweight, defensible ICTAI 2026 / CCF-C submission, not a large multi-month research program.

ICTAI 2026 target constraints:

- Submission deadline: 2026-06-30.
- Format: IEEE anonymous double-column conference paper.
- Full paper length: up to 8 pages.
- Practical objective: produce a reproducible experiment package and paper draft that can be submitted if results support the claim.

Do not include the project owner's identity, SJTU affiliation, or publication history in the anonymous submission draft.

## Mission

Build a lightweight, reproducible ICTAI submission around MiMo-based scientific idea generation on Sci-Reasoning. The main method is PGCR: Pattern-Guided Candidate Expansion and Reranking.

Read `Plan/00_BIG_PLAN.md` first, then read only the current phase plan named in `Plan/RUN_STATE.md`.

## Ground Truth Priority

Use sources in this order:

1. Sci-Reasoning paper PDFs.
2. Sci-Reasoning repository code and result JSON files.
3. HuggingFace `AmberLJC/Sci-Reasoning` files.
4. Official ICTAI pages.
5. Local `wiki/` and `raw/` paper summaries.

## Research Integrity Rules

- Do not fabricate results.
- Do not fabricate citations, DOI values, arXiv IDs, author names, venues, or page numbers.
- Every number used in the paper must be traceable to a result file.
- Generation and reranking prompts must not see the target title or target contribution.
- Judge prompts may see the target title and contribution only after ideas are generated.
- If a result is weak, report it honestly and pivot the framing instead of inflating claims.

## Scope Rules

- Keep changes small and task-driven.
- Prefer new files under `scripts/`, `data/`, `results/`, `logs/`, and `paper/`.
- Do not refactor the original `Sci-Reasoning/` repository unless a small compatibility wrapper is impossible.
- Do not implement fine-tuning unless the user explicitly approves it.
- Do not build a full graph database unless a phase plan is updated with evidence and user approval.

## Automation Rules

- Before a long run, run a 1-3 item smoke test.
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
- Do not install system packages. Use a project-local virtual environment or user-local package cache.

## Python Virtual Environment Rules
Use UV virtual environment, python version=3.12

## MiMo Rules

Expected environment variables(in .env):

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

## HuggingFace Ruls
First use environment variable HF_MIRROR in the .env file.

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

- Use IEEE anonymous conference format.
- Remove author-identifying paths and institution text from the submission version.
- Keep all paper claims tied to experiment outputs.
- Leave AI-use disclosure as a final author decision, but do not forget IEEE policy constraints.

