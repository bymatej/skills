# Agent Instructions

This repository stores reusable AI agent skills.

## Repository Shape

- Put all installable skills under `skills/`.
- Each skill lives in its own directory and must include a `SKILL.md`.
- Prefer `skills/<category>/<skill-name>/SKILL.md` for original skills.
- Vendored upstream skills live under `skills/vendor/<source>/`.
- Use `references/`, `scripts/`, `assets/`, or `agents/` inside a skill only when they reduce repeated work or improve reliability.
- Keep `scripts/`, `docs/`, and `vendor/` focused. They are support directories, not dumping grounds.

## Skill Standards

Each `SKILL.md` should include:

- YAML frontmatter with `name` and `description`.
- A concise trigger description.
- A clear workflow for the agent.
- References to extra files only when needed.

Skills should be concise, focused, composable, and agent-oriented. Prefer portable markdown-based skills over agent-specific formats unless a skill has a real agent-specific need.

## Compatibility

Keep compatibility with Codex, Claude, Claude Code, and other agents supported by the `skills` CLI in mind.

`CLAUDE.md` is a symlink to this file so Claude and Codex receive the same repository instructions.

## Maintenance Rules

- When adding, removing, renaming, or changing a skill, update `README.md` if the change affects users.
- When changing repository structure, install flow, compatibility rules, or maintenance process, update this file.
- Avoid adding extra documentation files unless they serve a clear purpose.
- Keep copied third-party skills attributed and preserve their license context.
- If vendored skills are synced from upstream, update the README with the upstream source and commit.

