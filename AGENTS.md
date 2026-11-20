# AGENTS.md

This repo packages the `shorts-guidelines` agent skill, plus the `prs-fixes` and `prs-reviews` PR helper skills. Agent skill for a daily autonomous two-agent routine on the publish-shorts project.

## Layout

- `skills/<skill>/SKILL.md`: one folder per skill. Frontmatter holds `name` (equal to the folder name) and `description` (says when to use the skill).
- `skills/shorts-guidelines/references/`: long reference material, loaded only when needed (only if present).
- `README.md`: what the repo is and how to install it.

## Rules for changes

- Write everything in EN-US. No em dashes or en dashes anywhere.
- Keep `SKILL.md` short and move long material to `references/`.
- Do not add dependencies or tooling the skill does not need.
- Never commit secrets, tokens, personal data or machine-specific paths.
- Never push to `main`. Open a pull request. `main` needs one approval, and the repo owner can bypass it.
- Commit messages and PR titles use Conventional Commits with a scope, for example `fix(shorts-guidelines): tighten the trigger description`.

## Check before opening a PR

```bash
grep -rnE $'\xe2\x80\x94|\xe2\x80\x93' --exclude-dir=.git .   # em and en dashes: must print nothing
head -5 skills/*/SKILL.md                                       # frontmatter intact
```
