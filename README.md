# agents-md

A generalized personal `AGENTS.md` template for Codex-style coding agents.

This repository is intended to hold shareable cross-repo defaults: git hygiene,
pull request discipline, public artifact safety, code quality preferences,
writing guidance, and collaboration rules.

## Use

Copy `AGENTS.md` to your global Codex config location:

```sh
cp AGENTS.md ~/.codex/AGENTS.md
```

Then add repository-specific guidance in the nearest scoped `AGENTS.md` inside
each project. Scoped files should override these global defaults when they are
more specific.

## Customize

Before sharing or using this file, adjust:

- branch naming conventions
- pull request title prefixes
- message signing format
- CI expectations
- any organization-specific security or publication rules

Keep this file concrete. Prefer exact commands, file names, owners, and project
terms over generic policy language when adding rules for a specific team.
