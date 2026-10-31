---
name: implement
description: Build a spec's sub-issues one by one in this session, or an ad-hoc task after a plan, then review the result with Opus subagents.
disable-model-invocation: true
argument-hint: <#spec | #issue | description>
---

Build the work in this session, one **slice** at a time: write the code, clean it up, commit it and close its issue. Then review the whole result with Opus subagents and settle the findings with the user.

## Steps

1. **Resolve the input.** Read `docs/agents/issue-tracker.md` (when it is missing, tell the user to run `/pet:setup` first and stop), then:
   - **A spec with sub-issues**: the queue is its open sub-issues, in order.
   - **An issue without sub-issues**: the queue is that issue.
   - **A plain description**: the queue is that task, with no issue.

   Size each item: **tiny** when it changes a few lines and adds no behaviour, **normal** otherwise. Say the sizes in chat; the user can overrule them.

2. **Check the working tree.** When `git status` shows changes, list them and ask the user whether to commit, stash or leave them. Changes left in place are never staged with a slice.

3. **Plan ad-hoc work.** When the queue item lacks the `ready-for-agent` label (an issue not refined by `/pet:spec`, or a description), say so, read the code and write a short plan in chat: files to change, approach, verification. When the issue looks bigger than one slice, offer `/pet:spec #<n>` as the alternative. Wait for the user's approval. The issue stays unlabelled, since the plan lives only in this session. Issues labelled `ready-for-agent` already are the plan.

4. **Branch.** Follow the branch rules in the project's `CLAUDE.md`. When the queue holds more than one sub-issue and pushing the current branch deploys (per those rules), propose `feature/<spec-slug>` off the current branch. On yes, create it and record its base: `git config branch.feature/<spec-slug>.base <current-branch>`. Single-slice and ad-hoc work stays on the current branch.

5. **Build each slice.** Take the next queue item (for a spec, the next sub-issue whose blockers are all closed; see **Next sub-issue** in `docs/agents/issue-tracker.md`), then:
   1. **Check for drift.** Skip this for an approved plan from step 3. Compare the issue's Technical notes with the current code and the project's coding standards (`CODING_STANDARDS.md`): the files, helpers, names and patterns it relies on, especially ones earlier slices or other commits changed since it was written. When something moved, note it in chat. When a note conflicts with a standard, ask the user which wins before building. When the issue's approach no longer works, remove `ready-for-agent`, comment on the issue what no longer works, stop and tell the user.
   2. **Build** exactly what the issue or approved plan asks. When a detail turns out unclear or wrong, ask the user instead of guessing.
   3. **Clean up.** For a normal slice, call the Skill tool with `simplify`, then with `pet:code-comments`, on the slice's changes. Pass `simplify` the issue's Technical notes as fixed decisions, so its reviewers treat them as settled. Run the Code Quality checks in `CLAUDE.md` and fix what they report.
   4. **Commit** a sub-issue of a spec without asking: stage only the slice's files, use an imperative title matching the repo's history and a `Refs #<n>` line in the body, then close the sub-issue with the comment `Done in <sha>`. An issue without sub-issues, or a description, stays uncommitted, so the review's fixes land in the same commit.

   Keep going until the queue is empty, or every open sub-issue is blocked by something outside it (report which). After the session is compacted, re-read the spec and the next sub-issue from the issue tracker rather than building from the summary.

6. **Review.** Follow `review.md` in the `check` skill's folder (`../check/review.md` from this skill's folder) with:
   - **issues**: the spec and its sub-issues, the single issue, or the description and its approved plan;
   - **base**: `HEAD` for a single issue or a description, since its work is uncommitted; otherwise as in **Base** in `review.md`;
   - **covered**: the Code Quality checks pass, and `simplify` and `code-comments` ran on every normal slice.

   When all the work is tiny, skip `review.md` steps 1–3: say so, commit any uncommitted work with an imperative title matching the repo's history (plus `Refs #<n>` and closing the issue when there is one), then follow `review.md` steps 4 and 5.
