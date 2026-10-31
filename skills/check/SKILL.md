---
name: check
description: Review unpushed work done outside /pet:implement against its spec and code quality, then fix or file the findings.
disable-model-invocation: true
model: opus
argument-hint: "[#spec | base ref]"
---

Review unpushed work (its commits, its uncommitted changes, or both) with the same review `/pet:implement` ends with: two axes, **spec** and **quality**, then every finding settled with the user: fix now, new issue, or ignore. Use it for work done outside `/pet:implement`; `/pet:implement` already reviews its own.

## Steps

1. **Pin the scope.** When `docs/agents/issue-tracker.md` is missing, tell the user to run `/pet:setup` first and stop. Run `git add -N` on untracked files first, so `git diff` shows new files. Find the issue: the `#<n>` argument, or the newest `Refs #<n>` among the unpushed commits (as defined in **Base** in `review.md`; on a feature branch, the commits since its recorded base `git config branch.<branch>.base`). With neither, ask the user which issue the work belongs to, if any. The base is a base ref argument when given, otherwise as in **Base** in `review.md`. List any commit in `git log <base>..HEAD` without `Refs #<n>`, and any uncommitted file that looks unrelated to `#<n>`, and ask whether to keep each in scope; run `git reset <file>` on an untracked file the user drops. Stop when the in-scope diff is empty.

2. **Fetch the issues.** Fetch issue `#<n>`; when it is a sub-issue, also fetch its parent spec.

3. **Run the checks.** Run the Code Quality checks in `CLAUDE.md` for each app the scope touches. When they fail, list the failures and ask whether to fix them before the review.

4. **Review.** Follow `review.md` in this skill's folder with those issues, that base, the files and commits the user took out of scope, and covered: the checks that passed.
