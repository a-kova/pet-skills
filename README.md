# pet-skills

A Claude Code plugin for solo developers building pet projects. It turns an idea into a GitHub spec, builds it in small commits, and reviews the work before you push.

`/pet:spec` interviews you until you've made every decision, then files the plan as a GitHub issue. `/pet:implement` builds and reviews it, and you push.

```
/pet:spec  →  /pet:implement  →  push
```

## Why pet

**pet** is inspired by [Matt Pocock's skills](https://github.com/mattpocock/skills). Matt's collection is great, but it covers more than a solo developer needs for a side project, so **pet** keeps only the spec, build and review loop.

## Install

Install the plugin once per machine:

```bash
claude plugin marketplace add a-kova/pet-skills
claude plugin install pet@pet-skills
```

Then open a Claude Code session in your repo and run `/pet:setup`. It asks a few questions, then shows a draft of the config files it will add.

## Skills

| Skill | Model | What it does |
| --- | --- | --- |
| `/pet:setup` | Your session | Writes the project files below and creates the GitHub labels. |
| `/pet:spec <idea or #issue>` | Opus | Interviews you and reads the code the change touches. After you approve a summary, it publishes a spec issue with technical notes, split into sub-issues when it's big. |
| `/pet:implement <#spec, #issue or description>` | Your session, Opus reviewers | Builds each sub-issue, cleans it up with `simplify` and `code-comments`, commits it and closes the issue. Without technical notes, it writes a plan for your OK first. Two reviewers then check the result against the spec and for code quality, and you approve one table of findings. |
| `/pet:check [#spec or base]` | Opus | Runs the Code Quality checks and the same review on work you did without `/pet:implement`. |
| `/pet:retro` | Your session | Proposes improvements to the skills, `CLAUDE.md` and `docs/agents/*`. |

## Where to start

| Situation | Start with |
| --- | --- |
| A feature or change bigger than a one-liner | `/pet:spec <idea>` |
| An idea for later | A one-line GitHub issue, then `/pet:spec #<issue>` |
| A small, clear fix | `/pet:implement <description>`, or just ask |
| Changes you made by just asking | `/pet:check` before you push |

## Project files

`/pet:setup` writes these. Edit them afterwards as you like.

- `CLAUDE.md` lists the Code Quality checks, the testing bar, which branches deploy, and what `/pet:spec` should ask about.
- `CODING_STANDARDS.md` holds the rules every change follows.
- `docs/agents/issue-tracker.md` explains how to work with GitHub issues through `gh`.
- `docs/agents/labels.md` lists the type labels plus `needs-design`, `ready-for-agent` and `wontfix`.
- `docs/agents/domain.md` says where the glossary and ADRs live.

## Third-party skills

| Skill | Source | Used by |
| --- | --- | --- |
| [`grilling`](https://github.com/mattpocock/skills/tree/v1.3.2/skills/productivity/grilling) | Matt Pocock | `/pet:spec` |
| [`writing-for-agents`](https://github.com/mattpocock/skills/tree/v1.3.2/skills/productivity/writing-for-agents) | Matt Pocock | `/pet:spec`, `/pet:retro` |
| [`domain-modeling`](https://github.com/mattpocock/skills/tree/v1.3.2/skills/engineering/domain-modeling) | Matt Pocock | Claude, when you discuss `GLOSSARY.md` or an ADR |
| [`simplify`](https://code.claude.com/docs/en/skills) | Claude Code | `/pet:implement` |

All of them ship with the plugin or with Claude Code.

## Development

Run `claude --plugin-dir <path to this repo>` to load the plugin from disk. Bump `version` in `.claude-plugin/plugin.json` with every change so `claude plugin update pet@pet-skills` picks it up. [CREDITS.md](CREDITS.md) explains how to pull in Matt's upstream changes.
