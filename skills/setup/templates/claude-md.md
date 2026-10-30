# Code Quality

After any code change, run lint, type-check and tests in each app the change touches before reporting the task as done:

```
<one line per app: cd <app> && <lint> && <type-check> && <format check> && <tests>>
```

Which tests to write is decided while planning: `/pet:spec` names them in the spec's Verification section (an ad-hoc plan in `/pet:implement` does the same), and only those get written. This is a pet project, so the bar is low: test only essential core logic, plus a regression test for every bug fix. UI, wiring and glue code get none.

---

# Git

I'm a solo developer, so there are no pull requests: a branch merges into its base locally and is pushed. Pushing needs my explicit go-ahead.

<What pushing each branch deploys, or: Pushing deploys nothing. All work goes on `main`.>

---

## Agent skills

### Workflow

`/pet:spec` → `/pet:implement` → push; `/pet:check` reviews work done outside `/pet:implement`. See https://github.com/a-kova/pet-skills.

### Issue tracker

<One line: where issues live.> See `docs/agents/issue-tracker.md`.

### Labels

`feature` / `bug` / `chore`, plus optional `needs-design`, `ready-for-agent` and `wontfix`. See `docs/agents/labels.md`.

### Domain docs

<Single-context or multi-context.> See `docs/agents/domain.md`.

### Review skills

<Optional: skills the quality reviewer calls on every diff, e.g. a framework review skill. Delete this section when there are none.>
