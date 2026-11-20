---
name: shorts-guidelines
description: Use when running the daily autonomous publish-shorts routine. Launches 2 agents in parallel (software engineering audits yesterday's run and fixes code; social media analyzes insights and proposes engagement levers), opens PRs, merges, bumps, releases and deploys. Trigger on /shorts-guidelines, "daily run", "shorts routine".
---

# /shorts-guidelines

Daily routine for the `publish-shorts` project (cwd `~/Workspace/publish-shorts`). Write in EN-US, with no emojis and no em dashes (use commas, colons, parentheses or periods).

## General rules

1. **Full autonomy:** no questions and no confirmation requests. Always pick the best recommendation and proceed.
2. **MCPs and plugins:** use them whenever they exist (GitHub MCP for PRs and CI, a memory plugin for history, Chrome via `mcp__claude-in-chrome__*` for any browser, never Safari).
3. **No time or token constraints:** no shortcuts, do the full analysis.
4. **Express authorization** (valid only for this session, never inherited by subagents): PR merge, version bump, manual release and deploy. The merge follows the exception in the repo's `CLAUDE.md`: only after green CI and a diff review, with a PR note saying the merge followed the session authorization. PRs that change the merge policy are left for the owner.
5. **Two agents in parallel**, launched in the same message (see below). The agents open the PRs but do not merge: the merge belongs to the main session.
6. Follow `/git-guidelines` (branch `<type>/<description>`, Conventional Commits with scope, PR with all template sections, EN-US), never commit straight to `main`, no force push, no Claude attribution. Follow `/programming-best-practices` (TDD first, types, docstrings, ruff, logging) and `/attack-protection`. Never read or print secrets from `.env` or `*token*.json`.

The `/daily-review` skill already runs `run-auditor` and `engagement-analyst` in parallel and produces `reports/daily/YYYY-MM-DD.md`: invoke it for the diagnosis, then use the two agents below (with write permission) to implement the fixes and open the PRs.

## Agent 1: Software Engineering

Subagent type `run-auditor` (read-only, writes the report to `reports/audit/`), followed by implementation of the fixes.

- **Role:** software engineering specialist.
- **Goal:** assess the previous day's run (Europe/Lisbon) and find execution and code gaps; make the fixes and open a PR.
- Sources: `data/run.log`, `data/metrics.log`, `data/launchagent-*.log`, the DB `data/publish_shorts.db` (always `sqlite3 -readonly`), `scripts/audit_last_24h.py`, `src/publish_shorts/`.
- Fixes with TDD (a failing test, then the fix), on a dedicated branch and an isolated worktree. The agent saves the report to a path that outlives the worktree (for example by copying it to the main repo or to the vault) so it is not lost.
- If there is no real gap, do not invent a PR: return the clean report.

## Agent 2: Social Media

Subagent type `engagement-analyst` (read-only, writes to `reports/engagement/` and `reports/experiments/log.md`), followed by implementation.

- **Role:** social media specialist.
- **Goal:** assess the previous day's insights and find programmatic ways to raise engagement on YouTube, Instagram, Facebook and TikTok; implement and open a PR.
- Respect experiments in progress in `reports/experiments/log.md` (do not contaminate open EXPs) and the existing feature flags. Propose a lever only with evidence and a large enough sample; otherwise record it as a hypothesis.
- If there is no safe lever, do not invent a PR.

## Closing (main session)

1. Wait for both agents. For each open PR: check CI (`gh pr checks`), review the diff, comment the authorization note, `gh pr merge --squash`.
2. CI bumps `VERSION` and publishes the release. Confirm the release and that the production clone (launchd, `scripts/run_hourly.sh`) is on `origin/main` at the new version.
3. Persist to the vault `~/Documents/Obsidian Vault/publish-shorts/`: the run note (`run-audit-YYYY-MM-DD.md`, `engagement-report-YYYY-MM-DD.md`) and a line in `MEMORY.md`.

## End condition

Deploy done with the fix in production. If neither agent finds anything to fix, just summarize everything on screen (run, engagement per platform, decisions and next milestones), with no PR and no deploy.
