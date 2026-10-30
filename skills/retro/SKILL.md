---
name: retro
description: Look back at a session and propose improvements to the workflow skills, CLAUDE.md and docs.
disable-model-invocation: true
argument-hint: "[session]"
---

Find what made this session slower or riskier than it needed to be, and propose changes to the **environment** (skills, `CLAUDE.md`, `docs/agents/*`, automated checks), never to the product code. Every added line is one more the user reads and keeps true, so propose few edits, and prefer deleting a line to adding one.

## Steps

1. Read the session: the current one unless the user names another. Past sessions are JSONL transcripts in `~/.claude/projects/<project-path-with-dashes>/`; pick by date or by the first user message.

2. Collect candidates, each tied to a moment in the session:
   - **Friction**: a workflow skill step that was skipped, repeated, or needed the user to correct it. → sharpen that step or its completion criterion.
   - **Missed mistake**: an error that reached review or the user. → prefer an automated check (lint rule, type, test, CI job) over a written rule.
   - **Missing fact**: something you had to search for or ask about. → a pointer in `CLAUDE.md` or a line in `docs/agents/*`.
   - **No-op or stale instruction**: a line in `CLAUDE.md`, a doc or a skill that changed nothing, or describes something no longer true. → delete it.

   Keep a candidate only when it would have changed how this session went and the situation will come back. No candidates is a good result: say so and stop.

3. Shape each edit. Call the Skill tool with `pet:writing-for-agents` first. Run `git log -p -5 -- <file>` on each file you would edit: when recent commits already patched the same clause, the clause is the problem, so rewrite or delete it rather than patch it again. Find the clause that already owns the decision and rewrite it, rather than adding a sentence beside it. A fact about one caller lives in that caller, never as a "when X" clause in a shared file. Aim for a batch that adds no lines; when a file grows, say what the new text replaces, or why nothing could.

4. Present the candidates, highest impact first, in plain words the user follows without knowing the skills' internals. For each, one to three sentences: what happened (the moment, quoted when you can), what it cost the user, and what will change. Name a skill step or term only with what it does. End with the net line change per file. Once the user picks, apply those edits and show their diffs. Edits to the workflow skills go to the `pet` plugin's source repo, never to the installed copy under `~/.claude/plugins/`, which an update replaces. Its path is in `~/.claude/CLAUDE.md`; when it isn't, ask the user once and add it there. In the source repo, bump the patch version in `.claude-plugin/plugin.json`.
