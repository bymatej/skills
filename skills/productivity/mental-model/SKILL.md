---
name: mental-model
description: Build a concise, evidence-grounded mental model from an RFC, design document, code change, GitHub pull request, large refactor, or system question. Use when the user asks for a mental model, meeting card, change map, system explanation, RFC compression, architecture flow, PR explanation, PR review, or help understanding how part of a codebase works. Do not use for implementation-only requests or generic summaries where no working mental model is requested.
---

# Mental Model

Create a compact representation the user can reconstruct and explain. Show what the subject is, why it exists, how it works, what state changes, what must remain true, why key decisions were made, what can fail, and how correctness is verified. Do not produce a generic long summary.

## Discover the source

Infer one or more modes from the request and available context:

- `RFC_MODE`: RFC or design document.
- `PR_MODE`: pull request number or URL.
- `CODE_CHANGE_MODE`: branch, commit, diff, or uncommitted changes.
- `SYSTEM_MODE`: subsystem, feature, process, data flow, or repository question.
- `REVIEW_MODE`: explicit request to review a PR or change; combine with the relevant source mode.

Ask only when the source cannot reasonably be identified. Discover available context from files, Git metadata, repository tools, or GitHub access before asking. Load only the matching workflow from [references/source-workflows.md](references/source-workflows.md). For a review, build the mental model before evaluating the change.

## Build the model

1. Establish the investigation boundary and semantic complexity.
2. Gather primary evidence. Treat descriptions, tickets, comments, and generated summaries as claims to verify.
3. Identify the semantic spine: entry point, orchestration, domain decision, state or external effect, and verification.
4. Track important claims as `Fact`, `Inference`, `Unknown`, or `Decision required`.
5. Preserve source terminology. Never invent architecture, intent, rules, rationale, or guarantees.
6. Write `Unknown` when evidence is insufficient and name the evidence that would resolve it.
7. Select only relevant sections from [references/output-template.md](references/output-template.md).
8. Validate the result with [references/quality-gates.md](references/quality-gates.md).

## Control depth

Choose depth by semantic complexity, not file count:

- **Small:** one coherent behaviour and few boundaries; Layer 0 plus concise Layer 1 and relevant references; about 700–1,200 words; at most one diagram.
- **Medium:** several components or one important API, database, queue, or deployment boundary; about 1,200–2,000 words and one or two diagrams.
- **Large:** several boundaries, rollout or migrations, mixed refactoring and behaviour, or a broad RFC/system; about 2,000–3,500 words, two to four purposeful diagrams, collapsible detail, and explicit coverage.

These are targets. Do not inflate simple subjects or omit critical risk to meet them.

## Write for retrieval

- Put a stand-alone, seven-field, roughly 120-word meeting card first.
- Keep Layer 1 reviewable in about five minutes.
- Prefer labelled fields, concise bullets, short tables, and one idea per line.
- Keep paragraphs to three sentences or fewer and avoid repeating layers.
- Put substantial detail in collapsible Layer 2 sections.
- Use Mermaid only to answer a concrete question. Use portable `flowchart TD`, `sequenceDiagram`, `stateDiagram-v2`, or `erDiagram` syntax; source-backed terms; short labels; and no decorative nodes.
- Include evidence references, analysis boundaries, confidence, rehearsal questions, a hidden answer key, and a five-line reconstruction prompt.
- Score confidence from `0.00` to `1.00` based on evidence quality. Reduce it for incomplete sources, partial inspection, static-only inference, unrun tests, undocumented intent, or conflicting sources.

## Write or update the artifact

Unless the user specifies otherwise, write under `docs/mental-models/` using:

- RFC: `<source-name>-mental-model.md`
- PR: `pr-<number>-<short-slug>-mental-model.md`
- code change: `<branch-or-change>-mental-model.md`
- system: `<topic-slug>-mental-model.md`

Do not overwrite an existing document casually. Update it deliberately while preserving useful content, or choose a versioned name. For a focused request, return or update only the requested section. When updating, read the current document, inspect new evidence, change affected sections, record the change, and recalculate confidence without rewriting unrelated prose.

## Finish

Return the artifact path, the meeting card, the most important unknown or risk, and a concise coverage statement. Mention commands actually run and relevant results. Never claim full review when coverage was partial, equate passing tests with correctness, or approve a pull request on the user's behalf.

Use [references/eval-prompts.md](references/eval-prompts.md) when evaluating changes to this skill.
