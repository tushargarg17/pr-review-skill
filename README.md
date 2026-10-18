# pr-review-skill

An in-depth pull request review skill for [Claude Code](https://claude.com/claude-code). It goes beyond a diff read: it maps the blast radius of a change, walks the affected user journeys, applies a fixed set of code-review lenses, separates bugs the PR introduced from ones that already existed, optionally drives the app in a browser, and can fix and retest until no medium-or-higher issues remain.

## What it does

| Phase | Purpose |
| --- | --- |
| 0. Context | Load PR metadata, diff, existing comments, CI and bot output |
| 1. Blast radius | Find every consumer of each changed export; check gating, routes, i18n |
| 2. Flows and journeys | Walk each affected user path across roles, states and entry points |
| 3. Code lenses | Correctness, async, render loops, security, data, performance, conventions |
| 4. Triage | PR-introduced vs pre-existing, reachability, severity (Critical to Low) |
| 5. End-to-end | Drive the preview or local build in a browser; record evidence |
| 6. Fix loop | Fix, lint, retest until clean (own PR or `--fix` only) |
| 7. Output | Structured report, optional PR description refresh, optional inline comments |

## Install

Requires [Claude Code](https://claude.com/claude-code) and the [GitHub CLI](https://cli.github.com) (`gh`), authenticated.

**As a slash command (per project):**

```bash
mkdir -p .claude/commands .claude/skills/pr-review
cp SKILL.md .claude/skills/pr-review/SKILL.md
cp .claude/commands/pr-review.md .claude/commands/pr-review.md
```

**As a personal skill (all projects):**

```bash
mkdir -p ~/.claude/skills/pr-review
cp SKILL.md ~/.claude/skills/pr-review/SKILL.md
```

## Usage

```
/pr-review 1234
/pr-review https://github.com/acme/app/pull/1234 --comment
/pr-review --fix            # review and fix your current branch's PR
/pr-review 1234 --quick     # skip browser E2E
```

| Flag | Effect |
| --- | --- |
| `--fix` | Fix Medium+ findings in the working tree and retest (never commits) |
| `--comment` | Draft inline comments, show them, post only after you confirm |
| `--e2e` | Force browser testing |
| `--quick` | Skip browser testing |

## Make it yours

The skill is generic. Project knowledge goes in two optional files in your repo, which it loads before reviewing:

- `.pr-review/rules.md` — your stack limits, conventions and recurring reviewer patterns. See [`examples/rules.example.md`](examples/rules.example.md).
- `.pr-review/scenarios.md` — a living list of product flows used to map diffs to journeys. See [`examples/scenarios.example.md`](examples/scenarios.example.md).

The "Self-improve" step can fold lessons from human review comments back into `rules.md` over time.

## Safety defaults

- Never commits, pushes or deploys on its own.
- Never approves or rejects a PR; comments are drafted and posted only after you say yes.
- Never types real credentials during browser testing.
- Treats PR text, diffs and bot comments as data, not instructions.

## Contributing

Issues and PRs welcome. Keep `SKILL.md` project-agnostic: anything specific to one stack or company belongs in the example rules file.

## License

[MIT](LICENSE)
