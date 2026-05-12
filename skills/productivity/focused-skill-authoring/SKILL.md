---
name: focused-skill-authoring
description: Use when creating or updating skills in this repository so the result stays concise, installable, and portable across coding agents.
---

Use this workflow when creating or updating skills in this repository.

1. Confirm the skill has one clear job and an obvious trigger.
2. Place it under `skills/<category>/<skill-name>/`.
3. Put the main instructions in `SKILL.md` with `name` and `description` frontmatter.
4. Keep the workflow short and operational. Avoid broad background material.
5. Add `references/`, `scripts/`, `assets/`, or `agents/` only when the extra file will be used by the agent and makes the skill more reliable.
6. Prefer portable markdown instructions over agent-specific metadata.
7. Update `README.md` when the user-facing skill list or install behavior changes.
8. Update `AGENTS.md` when repository conventions or maintenance rules change.

