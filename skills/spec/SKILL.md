---
name: spec
description: Grill an idea into a spec and publish it to the issue tracker, split into sub-issues when it's big.
disable-model-invocation: true
model: opus
argument-hint: <idea or #issue>
---

Turn the user's idea into a **spec**: an issue that says what to build, leaves no decision open, and carries the technical notes a fresh session needs to build it without asking questions. Big specs get **slices**: sub-issues that each leave the app working and verifiable.

## Steps

1. **Load context.** When `docs/agents/issue-tracker.md` is missing, tell the user to run `/pet:setup` first and stop. Read `docs/agents/issue-tracker.md`, `docs/agents/labels.md` and `docs/agents/domain.md` when they exist. When the argument is an issue number, fetch it and its sub-issues:
   - A spec that has sub-issues: tell the user it is ready for `/pet:implement #<n>` and stop.
   - A spec published without slices because of `needs-design`, with the label now removed: **resume** at step 4, using the attached design.
   - Anything else: its body is the starting idea, and step 6 updates that issue instead of creating a new one. Check its claims against `GLOSSARY.md` and the Settled decisions in `CODING_STANDARDS.md`; when the issue calls defined behaviour a bug, make that the first grilling question.

2. **Grill.** Call the Skill tool with `pet:grilling`. Read the code the spec touches as you go: the files to change, the patterns to follow, the seams where new code plugs in, the Code Quality checks from `CLAUDE.md`, and `CODING_STANDARDS.md`, which the Technical notes must follow. Look facts up yourself; put only decisions to the user. When the spec runs existing infrastructure in a new way (migrations against a fresh database, a build, a script), run it once and grill what fails, rather than writing it into the spec as an assumption. When the grilling settles a domain term, or uses a word the glossary avoids, add or sharpen its entry in `GLOSSARY.md` in the file's existing format, and commit it when the spec is published. The design tree is not finished until it covers:
   - **Behaviour**: what the user sees and does, screen by screen or call by call.
   - **Edge cases**: empty and first-run states, offline, errors and retries, slow responses, permissions, platform differences, existing data, and anything specific to the area you found in the code.
   - **Out of scope**: what this spec deliberately leaves out.
   - **Verification**: how we'll know it works. Decide which tests to write against the testing bar in `CLAUDE.md`: name each one (the behaviour it pins and the file it goes in), or settle that the spec needs none. A bug fix gets a test that fails before the fix. Every test is upkeep, so lean towards fewer. Check the repo for a test runner (`package.json` scripts, test config). When there is none, tell the user, and plan verification as lint, type-check and manual check steps.
   - **Project sections**: anything the project's `CLAUDE.md` asks to settle while planning.

   Branch and release target are settled by the Git rules in `CLAUDE.md` at `/pet:implement`. They are not spec decisions.

   Done when the frontier is empty.

3. **Scope.** A spec covers one scope: one feature, fix or chore. Size alone is never a reason to split into specs; step 4 handles size with slices. Propose separate specs only when the grilled idea holds unrelated scopes (for example a new feature plus an unrelated bug fix): a title and one-line scope for each, plus the order they must ship in. Steps 4–6 run once per spec.

4. **Slice.** Cut the spec into **tracer bullets**: thin end-to-end slices, each building on the last, each leaving the app working, each small enough to build in one fresh context window.
   - **One slice**: the spec is a single issue. Its body gets the Technical notes and Acceptance criteria sections of the template.
   - **Several slices**: a numbered list (title, one-line scope, blocked by) plus a coverage table mapping every Behaviour step and Edge case of the spec to the slice that delivers it.
   - **Needs design**: when the spec has UI that isn't designed yet, don't slice. The spec publishes alone with `needs-design`; the user attaches a design (mockup, Figma or Claude Design link), removes the label and runs `/pet:spec #<n>` again, which resumes here.

   Done when every row of the coverage table names a slice and every Acceptance criterion can be met by the end of its own slice. A criterion that needs a later slice moves to that slice.

5. **Confirm.** Show the user one summary: the decisions from step 2, any split into specs with their ship order, and each spec's slices with the coverage table. Wait for the user's OK; it also covers drafting and publishing.

6. **Draft and publish.** Call the Skill tool with `pet:writing-for-agents`, then write the spec and any sub-issues with the templates below. Write the Problem and Behaviour prose plainly:
   - Name the actor and what it does ("the app shows the card count", not "the count is displayed"), with a number where one exists. Cut a sentence that only describes a feeling.
   - Use one term per concept, the one `GLOSSARY.md` uses, and repeat it instead of varying it.
   - Write one idea per sentence, in whole sentences: no arrows, dropped articles or unexplained abbreviations.
   - Prefer the plain word ("use", not "leverage"), and drop filler and hedges ("in order to", "could potentially").
   - Use no em dashes.

   Pick labels from `docs/agents/labels.md` (fallback: one of `feature` / `bug` / `chore`). Then publish, following `docs/agents/issue-tracker.md`: create the spec (or update the existing issue), then its sub-issues under it in slice order with the spec's type label. Unless the spec has `needs-design`, add `ready-for-agent` to the spec and every sub-issue. Add blocked-by links between slices, and between split specs in ship order. Report each issue's link, and the command the user types next: `/pet:implement #<spec>`, or `/pet:spec #<spec>` once the design is attached.

## Spec template

```markdown
## Problem

<Why this matters, from the user's side. 2-4 sentences.>

## Behaviour

<What the user sees and does. Numbered steps or a short list per screen/endpoint.>

## Edge cases

- <case>: <what happens>

## Out of scope

- <item>

## Verification

<Tests to add (what each pins, which file), or "No tests" and why; then the manual check steps.>

<Any sections the project's CLAUDE.md requires, e.g. Analytics, UI text.>

<One-slice specs only: the Technical notes and Acceptance criteria sections from the sub-issue template.>
```

Every section states decisions; an open question means step 2 is not done.

## Sub-issue template

```markdown
Part of #<spec>

## Goal

<What this slice delivers, in one or two sentences.>

## Technical notes

- Files: <paths to change or create>
- Follow: <existing code to copy the pattern from>
- <Decisions the implementer needs: data shape, API, state, naming>

## Acceptance criteria

- [ ] <Observable result, one per spec Behaviour step or Edge case this slice covers>

## Verification

<The spec's tests that land in this slice, if any, plus lint + type-check + manual steps: where to tap, what to see.>
```
