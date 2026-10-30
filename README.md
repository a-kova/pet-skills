# pet

A Claude Code plugin with a lean workflow for solo pet projects: take an idea to a pushed branch with four skills, `spec`, `implement`, `check` and `retro`. They hold no project facts; each repo describes itself in a few files that `/pet:setup` writes.

```
/pet:spec  →  /pet:implement  →  push
```

`/pet:implement` ends with a review. `/pet:check` runs the same review on work done outside `/pet:implement`.

## Install

Once per machine:

```bash
claude plugin marketplace add a-kova/pet-skills
claude plugin install pet@pet-skills
```

Once per repo, run `/pet:setup`.

While working on the plugin itself, start a session with `claude --plugin-dir <path to this repo>` to load it from disk.

## Skills

| Skill | Model | What it does |
| --- | --- | --- |
| `/pet:setup` | Your session | Writes `docs/agents/*`, `CODING_STANDARDS.md` and the `CLAUDE.md` sections the other skills read, and creates the labels. |
| `/pet:spec <idea or #issue>` | Opus | Interviews you until every decision and edge case is settled, reading the code as it goes, then shows one summary for your OK and publishes a spec issue with a type label, technical notes and `ready-for-agent`. Big specs get sub-issues linked with "blocked by". With `needs-design`, it publishes the spec alone; run `/pet:spec #<n>` again once the design is attached. |
| `/pet:implement <#spec, #issue or description>` | Your session, Opus reviewers | Builds each unblocked sub-issue in turn, cleans it up (`simplify`, `code-comments`), commits it locally with `Refs #<n>` and closes the issue. Before each issue it checks the code hasn't drifted from the issue's technical notes. For an issue without technical notes, or a description, it writes a plan first and waits for your OK. At the end two Opus subagents review the whole result against the spec and code quality, and you approve one table of findings with a proposed action each (fix now, new issue, ignore). Tiny work (a few lines, no new behaviour) skips the cleanup and the review. |
| `/pet:check [#spec or base]` | Opus | For work done outside `/pet:implement`: runs the Code Quality checks, then the same review on unpushed commits and uncommitted changes, then does what you approve. |
| push | — | Yours. Say "push" or run `git push`. |
| `/pet:retro` | Your session | Now and then: proposes improvements to these skills, `CLAUDE.md` and `docs/agents/*`. Skill edits land in this repo, not the installed copy. |
| `/pet:improve-codebase-architecture` | Your session | Finds deepening opportunities, grills the one you pick, and publishes it as a `chore` spec. |
| `/pet:diagnosing-bugs <symptom>` | Your session | A diagnosis loop for a bug whose cause isn't obvious. |
| `/pet:domain-modeling` | Your session | Sharpens terms in `GLOSSARY.md` or records a decision in `docs/adr/`. |

`grilling`, `writing-for-agents`, `codebase-design` and `code-comments` are building blocks the skills above call; you can also call them yourself.

The review both `/pet:implement` and `/pet:check` run lives in `skills/check/review.md`, with its two reviewer briefs in `skills/check/reviewers/`.

## Where to start

| Situation | Start with |
| --- | --- |
| A feature or change bigger than a one-liner | `/pet:spec <idea>` |
| An idea for later | A GitHub issue (one line is fine). Pick it up with `/pet:spec #<issue>` |
| A small, clear fix | `/pet:implement <description>`, or just ask |
| A bug whose cause isn't obvious | `/pet:diagnosing-bugs <symptom>` |
| Changes made by just asking, before you push them | `/pet:check` |

## Notes

- **Commits**: `/pet:implement` commits each sub-issue of a spec locally without asking; you review the code once, at the review's table. A single issue or description stays uncommitted until the review, so its fixes land in the same commit. Otherwise the approved fixes land as one `Fix review findings` commit. Nothing is pushed without your go-ahead.
- **Branches**: a spec with several sub-issues gets a `feature/<slug>` branch when pushing the current branch would deploy, so half-done work never ships. The review offers to merge it back when the spec is done.
- **Context**: `/pet:implement` can start in a fresh session, because the issues hold what it needs.

## What a project provides

`/pet:setup` writes these; edit them directly afterwards.

- **`CLAUDE.md`**:
  - the Code Quality checks (lint, type-check, tests) and the testing bar;
  - which branches deploy when pushed, and where each kind of change goes;
  - anything to settle while planning (asked by `/pet:spec`);
  - optionally, review skills the quality reviewer calls.
- **`CODING_STANDARDS.md`**: the rules every change follows (used by `/pet:spec`'s technical notes, `/pet:implement`'s drift check, and the review).
- **`docs/agents/issue-tracker.md`**: how to create, read and link issues. GitHub through `gh` only.
- **`docs/agents/labels.md`**: the type labels plus `needs-design`, `ready-for-agent` and `wontfix`.
- **`docs/agents/domain.md`**: where the glossary and ADRs live.

## Dependencies

`simplify`, built into Claude Code, used by `/pet:implement`. Everything else ships in this plugin.

## Updating

Bump `version` in `.claude-plugin/plugin.json` with every change, so installs pick it up (`claude plugin update pet@pet-skills`). The skills under `skills/vendor/` are forks of Matt Pocock's; see [CREDITS.md](CREDITS.md) for how to pull in his changes.
