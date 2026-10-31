# Spec review

Check a diff against the issues it was built for and the project's rules. You get the repo path, the base, the issues (or a task description and plan), what is out of scope, and what is already covered.

Read `git diff <base>`, `git log <base>..HEAD --oneline` and the issues, skipping what is out of scope. With no issues, treat the task description and plan as the spec; with neither, review against the commit messages alone. Report:

- (a) requirements the spec asked for that are missing or partial;
- (b) behaviour in the diff the spec didn't ask for;
- (c) requirements that look implemented but wrong;
- (d) breaches of the rules the project's `CLAUDE.md` and `CODING_STANDARDS.md` set for every change.

Confirm each finding in the code and quote the spec line or rule for it. Leave out what covered already verified. Under 400 words.
