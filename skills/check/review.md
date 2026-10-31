# Review

The review that ends `/pet:implement` and makes up `/pet:check`. The caller provides:

- **issues**: the issue the work belongs to, plus its parent spec when it is a sub-issue; or, with no issue, the task description and the approved plan when there is one;
- **base**: the ref the scope starts from, found as in **Base** below unless the caller or the user named one;
- **scope**: `git diff <base>` (commits plus uncommitted changes) and `git log <base>..HEAD --oneline`, minus any file or commit the user took out of scope;
- **covered**: what the caller already verified on this scope, or none.

Approving the triage table is the user's permission to commit to the local branch.

## Base

The first that applies:

1. On a branch with a recorded base (`git config branch.<branch>.base`): that base.
2. The parent of the oldest **unpushed** commit whose body refs one of the issues, or of the oldest unpushed commit when there is no issue. Unpushed commits are `git log @{u}..HEAD`; on a branch with no upstream, `git log HEAD --not --remotes`. When that commit is the repo's first, the base is the empty tree (`git hash-object -t tree /dev/null`) and the log is `git log HEAD`.
3. `HEAD`, when none is committed yet.

## Steps

1. **Review.** Start two subagents with `model: opus` in one message, one per brief in this skill's folder: `reviewers/quality.md` and `reviewers/spec.md`. Give each the brief's full path, the repo path, the base, the issues (listing the spec's other open sub-issues by number and title) or the task description and plan, the files and commits out of scope, and covered. Wait for both reports.

2. **Triage.** Merge both reports and drop duplicates. Classify each finding:
   - **fix now**: about code or behaviour this diff introduces.
   - **new issue**: real, but about code this diff didn't touch, or too big for this change.
   - **ignore**: a judgement call not worth acting on, or behaviour that `GLOSSARY.md` or the Settled decisions in `CODING_STANDARDS.md` define as intended (say why, quoting the line).

   Show one table: `# | finding | file:line | source | action | why`, at the end of your final message. Wait for the user to approve or edit it.

3. **Act.**
   - **fix now**: make the fixes (for many or large ones, a `model: sonnet` subagent with the list) and rerun the Code Quality checks in `CLAUDE.md`.
   - **commit**: when the scope has uncommitted changes, stage the in-scope files only and commit them together with the fixes: an imperative title matching the repo's history and a `Refs #<n>` line in the body (omitted without an issue). Close the issue with the comment `Done in <sha>` when the change completes it. Otherwise commit the fixes alone as `Fix review findings` with a `Refs #<spec>` line.
   - **new issue**: create one issue per finding with a short body (what, where, why it matters) and a type label from `docs/agents/labels.md`.

4. **Close and merge.** When every sub-issue of the spec is closed, close the spec. On a branch with a recorded base whose spec is closed, offer to merge it into the base (`git checkout <base> && git merge --no-ff <branch>`) and delete the local branch (`git branch -d <branch>`).

5. **Report**, in plain words:
   - where the work is: the branch it sits on and its commits, with nothing pushed;
   - what is verified: the Code Quality checks and tests that passed;
   - what is not: the manual check steps from the spec's Verification (or steps you write when there is no spec), for the user to run before pushing;
   - anything still uncommitted in the working tree;
   - what pushing that branch deploys, per the project's `CLAUDE.md`.
