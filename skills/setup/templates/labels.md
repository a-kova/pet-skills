# Labels

The label strings the workflow skills apply to issues in this repo.

## Type labels (default)

Every issue carries exactly one.

| Label     | Meaning                                       |
| --------- | --------------------------------------------- |
| `feature` | New or changed behaviour for users            |
| `bug`     | Something is broken                           |
| `chore`   | Tests, dependencies, tooling, refactors, docs |

## Optional labels

| Label             | Meaning                                                                                                                                                                                       |
| ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `needs-design`    | The spec has UI that isn't designed yet. `/pet:spec` adds it and publishes the spec without sub-issues until it's removed. Remove it once a mockup, Figma, or Claude Design link is on the issue. |
| `wontfix`         | Will not be actioned. Close the issue with it.                                                                                                                                                |
| `ready-for-agent` | Refined: the issue carries Technical notes a fresh session can build from. Only `/pet:spec` adds it, when it publishes those notes. `/pet:implement` removes it when the notes no longer work.        |
