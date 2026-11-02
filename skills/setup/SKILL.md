---
name: setup
description: Configure this repo for the pet workflow skills: issue tracker, labels, domain docs, coding standards and the CLAUDE.md sections they read. Run once per repo.
disable-model-invocation: true
---

Write the per-repo files the workflow skills read. Explore first, show a draft, ask one question at a time with the recommended answer first, then write. Never overwrite text the user wrote: update a section in place, and leave a file alone when it already says what the template says.

## Steps

1. **Explore.** Read what exists; don't assume:
   - `git remote -v`: is the repo on GitHub? Then check `gh repo view` works from this folder.
   - `CLAUDE.md` (or `AGENTS.md` when there is no `CLAUDE.md`): which of the sections in [templates/claude-md.md](templates/claude-md.md) it already has, under any heading.
   - `docs/agents/`, `CODING_STANDARDS.md`, `GLOSSARY.md`, `GLOSSARY-MAP.md` and `docs/adr/`.
   - `gh label list`: which labels from [templates/labels.md](templates/labels.md) are missing.
   - Each app's lint, type-check, format and test commands, from its `package.json` scripts or equivalent build files. An app is the repo root or a workspace folder with its own manifest.
   - Branches that deploy when pushed: workflow files under `.github/workflows/` and their `on.push.branches`.
   - Monorepo signals: `pnpm-workspace.yaml`, a `workspaces` field, or `packages/*` with their own `src/`.

2. **Present findings and ask.** Summarise what is present and what is missing. Then ask only what exploration could not settle, one question per message, recommended answer first:
   - **Issue tracker**: GitHub through `gh` is the only tracker the skills support. When no remote points at GitHub, offer to create one (`gh repo create`) or add one; stop when the user declines.
   - **Code Quality commands**: confirm the commands found for each app. With no test runner, say so; the skills then verify with lint, type-check and manual steps.
   - **Testing bar**: keep the template's "essential core logic plus a regression test per bug fix"?
   - **Git**: does pushing any branch deploy something? Recommend what the workflow files show; with none, "nothing deploys, all work on `main`".
   - **Labels**: keep the defaults?
   - **Domain docs**: single-context without asking, unless monorepo signals exist; then ask single- or multi-context. `domain.md` covers both; the answer only sets the line in `CLAUDE.md`.
   - **Review skills**: the quality reviewer runs its own checks on every diff either way; should it also call extra skills? Recommend "Default (built-in review only)". Never label that option "None": it reads as no code review at all.

3. **Confirm.** Show the draft of every file and section you will write or change, as diffs against what exists. Let the user edit it, and wait for their OK.

4. **Write.**
   - `docs/agents/issue-tracker.md`, `docs/agents/labels.md` and `docs/agents/domain.md` from [templates/](templates/): as they are when missing; an existing one gets only the template text it lacks, as approved in step 3.
   - `CODING_STANDARDS.md` from [templates/CODING_STANDARDS.md](templates/CODING_STANDARDS.md), only when it is missing.
   - The sections of [templates/claude-md.md](templates/claude-md.md) into `CLAUDE.md`, filled with the answers. Update a section that already exists in place; add only the missing ones. When neither `CLAUDE.md` nor `AGENTS.md` exists, create `CLAUDE.md`.
   - `gh label create` for each missing label.
   - Offer to commit `.claude/settings.json` entries, so opening the repo on another machine offers the plugin: `"extraKnownMarketplaces": {"pet-skills": {"source": {"source": "github", "repo": "a-kova/pet-skills"}}}` and `"enabledPlugins": {"pet@pet-skills": true}`, merged into what is there.

5. **Report** the files written and the labels created, and that the next step is `/pet:spec <idea>`. The user edits these files directly later; running `/pet:setup` again is only needed to redo the setup.
