# Phase 0 Guide: Claude Code Automation Operations

## Objective

Run the project with Claude Code from Phase 1 onward while keeping context pressure, server risk, and research drift under control.

This is not a late-stage task. The user should not manually complete Phases 1-5 before involving Claude Code. Claude Code should read this guide first, then execute `Plan/01_SETUP_AND_DATA.md`.

## Recommended Workflow

Use this pattern:

1. Claude Code reads `CLAUDE.md`.
2. Claude Code reads `Plan/00_BIG_PLAN.md`.
3. Claude Code reads this automation guide.
4. Claude Code reads only the current phase plan from `Plan/RUN_STATE.md`.
5. Claude Code updates `Plan/RUN_STATE.md`.
6. Claude Code runs a small smoke test before full automation.
7. Long jobs must checkpoint and resume.

This is a local version of file-based planning: the context window is volatile; markdown files are persistent memory.

## Useful External Practices

### Karpathy-style autoresearch

Reference: https://github.com/karpathy/autoresearch

Useful ideas to copy:

- one fixed metric
- tight experiment loop
- comparable runs
- logs for every attempt
- keep/discard decisions based on measurements
- a small instruction file like `program.md`

Do not copy blindly:

- disabling all permissions on a shared server
- modifying arbitrary files
- optimizing without a stable evaluation

### Planning with Files

Reference: https://agentskill.sh/plugins/trailofbits/planning-with-files

Optional install:

```text
/plugin marketplace add trailofbits/skills-curated
/plugin install trailofbits/skills-curated/plugins/planning-with-files
```

Useful ideas:

- `task_plan.md` for phases
- `findings.md` for discoveries
- `progress.md` for session logs
- read-before-decide

This project uses:

- `Plan/00_BIG_PLAN.md`
- phase-specific plans
- `Plan/RUN_STATE.md`

Installing the plugin is optional.

If installed, use its read-before-decide discipline, but treat this project's `Plan/` directory as canonical. Do not create redundant root-level planning files unless the current phase explicitly needs them.

### Superpowers

References:

- https://claude.com/plugins/superpowers
- https://github.com/obra/superpowers

Useful skills:

- brainstorming
- writing plans
- execute-plan
- systematic debugging
- code review

Recommended if available:

```text
/plugin install superpowers@claude-plugins-official
```

Do not make the project depend on Superpowers. The plan files should be enough. If Superpowers is installed, use it for:

- brainstorming only when a replanning trigger fires
- execute-plan style phase execution
- systematic debugging for failing scripts
- code review before long runs or paper generation

### Claude Code Official Practices

References:

- Best practices: https://code.claude.com/docs/en/best-practices
- Memory / CLAUDE.md: https://code.claude.com/docs/en/memory
- Permission modes: https://code.claude.com/docs/en/permission-modes

Relevant official patterns:

- provide verification criteria
- explore first, then plan, then code
- use CLAUDE.md for project memory
- manage context aggressively
- use non-interactive mode for automation
- use auto or dontAsk modes carefully

## Permission Recommendations

On a shared lab server, do not run unrestricted `--dangerously-skip-permissions` in the host environment.

Preferred:

- container, dev container, VM, or dedicated low-privilege user
- project directory as the only write target
- explicit allowlist for network domains
- no sudo
- no system package changes

If using Claude Code permission modes:

- `plan`: for exploration and planning
- `acceptEdits`: acceptable inside project directory
- `auto`: acceptable if available and project is sandboxed
- `dontAsk`: useful only when allow rules are complete
- `bypassPermissions`: only inside isolated throwaway environments

## Server Files to Put in Place

Minimum server package:

```text
ICTAI/
  AGENTS.md
  CLAUDE.md
  Plan/
  Sci-Reasoning/
  raw/
  wiki/
  awesome-ai-auto-research/
  2601.04577v1.pdf
```

Recommended generated/empty directories:

```text
data/scireasoning/
results/setup/
results/runs/
logs/
paper/
scripts/
```

Environment variables:

```bash
export XIAOMI_MIMO_BASE_URL="https://..."
export XIAOMI_MIMO_API_KEY="..."
export MIMO_MODEL="mimo-v2.5-pro"
export MIMO_SLEEP_SECONDS="0.5"
```

Optional:

```bash
export HF_HOME="$PWD/.cache/huggingface"
export TRANSFORMERS_CACHE="$PWD/.cache/transformers"
```

## Automation Logging Rules

Every script must write:

- command arguments
- timestamp
- input file path
- output file path
- model name
- sleep seconds
- prompt template version
- completed target count
- failure count
- token usage if available

Every long job must:

- support `--resume`
- checkpoint after each target
- sleep at least 0.5 seconds between MiMo calls
- back off on 429/rate-limit errors
- avoid parallel MiMo workers unless the user explicitly approves it
- never lose completed results after an API failure
- write logs under `logs/`

Claude Code should not stop for ordinary missing optional data, minor format ambiguity, or recoverable API failures. It should choose the smallest documented fallback, log it, and continue. It should ask the user only for secrets, system-level changes, access outside the project directory, destructive deletion beyond generated caches, parallel MiMo execution, fine-tuning, full KG construction, or a major research-scope change.

## Suggested Claude Code Start Prompt

Use this on the lab server after files and environment variables are ready:

```text
Read CLAUDE.md first. Then read Plan/00_BIG_PLAN.md, Plan/06_AUTOMATION_AND_CLAUDE_CODE.md, and Plan/RUN_STATE.md.

If Superpowers is installed, use its execute-plan, systematic debugging, and code-review practices. If planning-with-files is installed, use its read-before-decide discipline, but treat the existing Plan/ directory and Plan/RUN_STATE.md as the canonical planning files. If either plugin is not installed, do not stop; continue with the local Plan workflow.

We are starting Phase 1 only. Read Plan/01_SETUP_AND_DATA.md and execute it end to end.

Hard constraints:
- Do not modify Sci-Reasoning source files unless needed for a small wrapper or compatibility fix; prefer new scripts under scripts/.
- Do not hardcode API keys.
- Do not use sudo or modify system directories.
- Do not access files outside this project directory.
- Every MiMo API call in experiment scripts must sleep at least 0.5 seconds by default because Claude Code and experiments share the same MiMo account.
- Do not parallelize MiMo calls unless the user explicitly approves after stable single-worker runs.
- If optional data is missing, use the smallest documented fallback and continue; ask only for secrets, system-level actions, destructive actions, outside-directory access, or major scope changes.
- Before any long network or API run, do a 1-item smoke test.
- At the end, update Plan/RUN_STATE.md with what worked, what failed, paths to outputs, and the next recommended phase.

Start by inventorying the environment and locating available Sci-Reasoning data. If something is missing, create a minimal documented fallback instead of stopping.
```

## Suggested Subsequent Phase Prompt

```text
Read CLAUDE.md, Plan/00_BIG_PLAN.md, Plan/RUN_STATE.md, and Plan/0X_PHASE_NAME.md.

Execute only this phase. Do not jump to later phases unless this phase's exit criteria are met.

Use small smoke tests first, then full runs. Update Plan/RUN_STATE.md when done.
```

## Context Management

When context gets large:

- summarize current state into `Plan/RUN_STATE.md`
- write detailed findings into `results/` or `logs/`
- start a fresh Claude Code session
- tell the new session exactly which plan file to read

Do not paste huge raw logs into chat. Store them in files and summarize paths.
