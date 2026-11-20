# shorts-guidelines

A daily routine for the `publish-shorts` project. Two agents run in parallel: one audits yesterday's run and fixes code, the other analyzes engagement insights and proposes levers. The main session then reviews and merges the PRs, confirms the release and deploy, and writes notes. It is tied to that project, so read it as a worked example of a multi-agent daily routine.

## Install

For Claude Code, copy the skill folders into your personal skills directory:

```bash
git clone https://github.com/orenatobr/shorts-guidelines.git
mkdir -p ~/.claude/skills
cp -R shorts-guidelines/skills/* ~/.claude/skills/
```

Start a new Claude Code session. Skills load from the `description` in each `SKILL.md`, so they trigger on their own and can also be called by name.

Other agents: each skill is a folder with a `SKILL.md` (YAML frontmatter plus Markdown). Tools that read the Agent Skills format can load `skills/` as it is. For the rest, paste the body of the `SKILL.md` into the agent's instruction file.

## Usage

Run `/shorts-guidelines` from the project directory. It also triggers on "daily run" and "shorts routine".

## Requirements

The `publish-shorts` project, the `run-auditor` and `engagement-analyst` subagents, the `/daily-review` skill, `gh` and the GitHub MCP.

## Related skills

It calls `git-guidelines`, `programming-best-practices` and `attack-protection`.

## What is in this repo

| Path | Purpose |
|------|---------|
| `skills/shorts-guidelines/` | The `shorts-guidelines` skill. |
| `skills/prs-fixes/` | PR helper: apply review feedback and resolve threads. |
| `skills/prs-reviews/` | PR helper: post a Requested Changes review. |
| `AGENTS.md` | Rules for AI agents that edit this repo. |

The two PR helpers ship in every repo of this family:

- `prs-fixes`: Applies the suggested fixes on one or more GitHub PRs, pushes them, replies to every review comment and resolves each thread. Example: `/prs-fixes 12, 15`.
- `prs-reviews`: Reviews one or more GitHub PRs and posts a Requested Changes review, one inline comment per finding, each with an exact location and a suggested change. Example: `/prs-reviews 12, 15`.

They need the `gh` CLI, and they call `git-guidelines`, `humanizer`, `programming-best-practices` and `attack-protection` when those skills are installed.

## Contributing

`main` is protected: changes land through a pull request with one approval. See [AGENTS.md](AGENTS.md) for the conventions.

## License

MIT, see [LICENSE](LICENSE).
