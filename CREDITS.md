# Credits

The workflow is modelled on [Matt Pocock's skills](https://github.com/mattpocock/skills) (MIT).

## Copied skills

`skills/vendor/` holds six of Matt's skills, copied from tag `v1.3.2` (commit `d1478fcb140db0b93c0c8df17bcfd311844749d0`), under his MIT license in [skills/vendor/LICENSE](skills/vendor/LICENSE):

| Skill | Upstream path | Changed here |
| --- | --- | --- |
| `grilling` | `skills/productivity/grilling` | no |
| `writing-for-agents` | `skills/productivity/writing-for-agents` | no |
| `domain-modeling` | `skills/engineering/domain-modeling` | no |
| `codebase-design` | `skills/engineering/codebase-design` | no |
| `improve-codebase-architecture` | `skills/engineering/improve-codebase-architecture` | step 4, Publish, added |
| `diagnosing-bugs` | `skills/engineering/diagnosing-bugs` | no |

The `agents/openai.yaml` files are left out; they are for Codex.

`skills/setup/templates/issue-tracker.md` and `domain.md` started from the templates in Matt's `setup-matt-pocock-skills`.

## Pulling in upstream changes

Check every couple of months:

1. Read Matt's `CHANGELOG.md` entries since the commit above, looking only for the six copied skills.
2. Port a change by hand when it fixes something you hit; skip the rest. These copies are forks, so overwriting them would undo the changes in the table.
3. Record the new tag and commit here, and bump the plugin version.
