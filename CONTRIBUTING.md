# Contributing skills

## Add a skill

1. Choose the app folder under `skills/`: `jessse`, `conclude`, `jettworth` or `narada`.
2. Add a directory named `<app>-<workflow>` containing `SKILL.md`.
3. Include YAML frontmatter with a unique `name` matching the directory and a concise `description`. Use lowercase letters, numbers and hyphens for names.
4. Describe real prerequisites, the workflow, tool permissions, recovery behavior and how to report results. Verify tool names against the app's current contract.
5. Add supporting files inside the skill directory only when they are needed. A skill must work when installed by itself: do not rely on another skill's relative filesystem location.
6. Update the app README and root catalog. Increment `metadata.version` when the instructions change.

Use the portable [Agent Skills format](https://agentskills.io/specification).
Keep generic instructions compatible across harnesses; label client-specific
setup when necessary. Do not include account credentials, private user data,
internal-only documents or app source code in this public catalog.

## Validate changes

From the repository root, confirm that the intended skills are discovered:

```bash
npx skills add . --list
```

Review frontmatter, links and any included scripts. For behavioral changes, test
the relevant workflow with the app's approved test environment and a compatible
agent. Verify permission failures and retry behavior where applicable; a successful
catalog listing alone does not prove a live workflow works.

## Source ownership

Maintain public skills here. Jessse's initial skills were imported from its
existing public skill endpoints. Those app-hosted copies still exist; while both
are distributed, coordinate changes explicitly to prevent drift. Moving the app's
download/install UI to this catalog is a separate app change.

Use short pull requests explaining the workflow change, affected apps, and checks
performed. Changes to a public skill must not silently broaden account permissions
or authorize actions beyond the user's request.
