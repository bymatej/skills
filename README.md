# Skills

Reusable AI agent skills for Codex, Claude, Claude Code, and other agents that understand `SKILL.md`-style instructions.

This repository keeps skills markdown-first and installable through the `skills` CLI where possible.

## Install

Install from GitHub with:

```sh
npx skills@latest add bymatej/skills
```

Install one skill with:

```sh
npx skills@latest add bymatej/skills --skill focused-skill-authoring
```

Install Matt Pocock's `grill-me` skill from this repo with:

```sh
npx skills@latest add bymatej/skills --skill grill-me
```

The `skills` CLI also accepts the full GitHub URL:

```sh
npx skills@latest add https://github.com/bymatej/skills
```

## Repository Layout

```text
.
├── AGENTS.md
├── CLAUDE.md -> AGENTS.md
├── README.md
├── docs/
├── scripts/
├── skills/
│   ├── productivity/
│   └── vendor/
│       └── mattpocock/
└── vendor/
    └── mattpocock-skills/
```

Original skills live under `skills/<category>/<skill-name>/`.

Copied upstream skills live under `skills/vendor/mattpocock/`. The upstream repository is also tracked as a Git submodule at `vendor/mattpocock-skills/` for inspection and manual sync.

## Add A Skill

Create a directory under `skills/<category>/<skill-name>/` and add `SKILL.md`:

```md
---
name: example-skill
description: Use when the agent should follow this specific workflow.
---

Describe the trigger and workflow here.
```

Keep the skill focused. Add `references/`, `scripts/`, `assets/`, or `agents/` only when they make the skill more reliable or easier to maintain.

Update this README when user-facing install commands, available skills, or behavior changes. Update `AGENTS.md` when repository conventions or maintenance rules change.

## Matt Pocock Skills

This repo reuses Matt Pocock's public skills from `mattpocock/skills`, which is MIT licensed. The upstream submodule currently points at:

```text
f304057d61d3df3c9fd992ac2b6e3833cb9325fb
```

The `skills` CLI does not reliably install skills through Git submodule contents or symlinked skill directories when reading from GitHub, so the upstream skills are copied into `skills/vendor/mattpocock/`.

Included upstream skill folders:

- `deprecated/design-an-interface`
- `deprecated/qa`
- `deprecated/request-refactor-plan`
- `deprecated/ubiquitous-language`
- `engineering/diagnose`
- `engineering/grill-with-docs`
- `engineering/improve-codebase-architecture`
- `engineering/prototype`
- `engineering/setup-matt-pocock-skills`
- `engineering/tdd`
- `engineering/to-issues`
- `engineering/to-prd`
- `engineering/triage`
- `engineering/zoom-out`
- `in-progress/review`
- `in-progress/writing-beats`
- `in-progress/writing-fragments`
- `in-progress/writing-shape`
- `misc/git-guardrails-claude-code`
- `misc/migrate-to-shoehorn`
- `misc/scaffold-exercises`
- `misc/setup-pre-commit`
- `personal/edit-article`
- `personal/obsidian-vault`
- `productivity/caveman`
- `productivity/grill-me`
- `productivity/handoff`
- `productivity/write-a-skill`

## Supported Agents

The current structure is intended for the `skills` CLI and markdown-based agent skill loaders. It should work with Codex, Claude Code, Cursor, GitHub Copilot, Windsurf, and other agents listed by the CLI, subject to each agent's own skill support.

## Limitations

- `package.json` is not required for `npx skills@latest add <owner>/<repo>`, so this repo does not include one yet.
- Vendored skills are copied from upstream rather than exposed through symlinks, because GitHub tree-based discovery cannot see symlinked or submodule skill contents as normal skill directories.
