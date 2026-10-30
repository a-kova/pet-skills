# Quality review

Review a diff for bugs, security and performance. You get the repo path, the base, the issues, and what is already covered.

1. Read `git diff <base>` in the repo, and enough of the surrounding code to judge each change. Look for what lint, type-check and tests miss:
   - **bugs**: wrong logic; empty, null, offline, retry and concurrency cases; silent failures;
   - **security**: unvalidated input, injection, auth or permission gaps, secrets in code or logs;
   - **performance**: N+1 queries, work repeated per render or request, unbounded lists or loops, leaked listeners, timers or subscriptions.

   When the project's `CLAUDE.md` names review skills, call each with the Skill tool on the same diff.
2. Confirm each finding in the code, and drop the ones that don't hold.
3. Leave out what covered already verified (`simplify`, for example, covers reuse and simplification).
4. Treat the spec's other open sub-issues as planned work: report a gap one of them covers only as "covered by #<n>".

Report each finding with file:line, severity and a one-line fix. Under 400 words.
