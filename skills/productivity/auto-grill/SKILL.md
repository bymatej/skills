---
name: auto-grill
description: >
  Stress-tests a plan against the real codebase without manual tab-switching.
  A griller agent produces adversarial questions blind to the code; a researcher
  agent answers them by actually reading models, migrations, tests, and context
  files; human judgment calls surface as A/B/C prompts. Use this whenever
  someone pastes a plan and wants it stress-tested, says "grill my plan", wants
  to find gaps before implementing, or invokes /auto-grill. Also trigger when
  the user says things like "does my design hold up?", "what am I missing?",
  or "sanity check this plan".
---

# Auto-Grill

Replaces the manual two-tab workflow: grill-me in one tab, codebase Q&A in
another. The griller stays adversarially blind to the code so questions are
generated from pure reasoning, not confirmation bias.

## Invocation

```
/auto-grill                              # prompts for plan
/auto-grill 15                           # 15 questions instead of 10
/auto-grill @path/to/file.py             # attach file as extra context
/auto-grill @plan.md @models.py 12       # multiple context files + count
```

`@<path>` args are read and passed to the researcher (not the griller).
A bare number sets the question count. Everything else is the plan inline.

---

## Step 1: Gather inputs

Parse args:
- `@<path>` tokens - read each file, label it by filename, collect as CONTEXT
- a bare integer - question count (default: 10)
- anything else - the plan

If no plan is found in args, ask the user to paste it.

Tell the griller only which context filenames exist - not their content.
This preserves the adversarial separation: the griller cannot see the answers.

---

## Step 2: Griller agent

Spawn a general-purpose agent with no codebase access. Give it the plan
and the list of context filenames (not contents).

The griller's job is adversarial: generate questions that would embarrass the
plan. It should not look anything up - only reason from the plan text.

```
You are a senior engineer adversarially reviewing a plan. Generate [N] hard
questions that stress-test assumptions, expose ambiguities, probe edge cases,
and challenge design decisions. Do NOT search the codebase or look anything
up - generate questions from pure reasoning about the plan.

Rules:
- Specific and concrete only (not "have you considered edge cases?")
- Cover: data model assumptions, state machine transitions, concurrency,
  failure modes, backwards compatibility, missing requirements,
  performance at scale, security boundaries
- Tag each question: [CODE] if answerable by reading code,
  [JUDGMENT] if it needs a human decision, [BOTH] if it needs both
- Number each question 1 through N
- Questions only - no answers

[If context files were attached]:
Note: reviewers have access to these files: [list filenames only]

Plan:
[PLAN TEXT]
```

---

## Step 3: Researcher agent

Spawn an Explore agent (or general-purpose with full tools). Give it the
[CODE] and [BOTH] questions, the plan, the working directory, and any
attached context files.

Skip [JUDGMENT]-only questions here - they go directly to the user in Step 4.

```
You are answering adversarial review questions about a plan. For each
question, search the actual codebase to find the real answer. Read models,
migrations, existing code, tests, and context files - do not guess.

Output format for each question:
**Q[N]**: [question text]
**Answer**: [what you found in code/docs/context]
**Confidence**: HIGH / MEDIUM / LOW
**Gap**: YES / NO
**Notes**: [if confidence < HIGH or gap is YES, explain what is missing
           or contradictory]

Codebase root: [working directory]

Check these context files first if they exist:
- lib/ai-codebase-context/_common/index.md
- lib/ai-codebase-context/_common/state-machines.md
- CLAUDE.md

[If attached context]:
Additional context provided by user:
[CONTEXT FILE CONTENTS WITH FILENAMES AS HEADERS]

Questions to answer:
[GRILLER OUTPUT - CODE and BOTH questions only]

Plan:
[PLAN TEXT]
```

---

## Step 4: Human decision points

After the researcher returns, collect every question that needs human judgment:
- All [JUDGMENT]-tagged questions
- All [BOTH] questions where researcher returned Confidence < HIGH or Gap = YES

For each decision, use `AskUserQuestion` (or plain-text prompt if unavailable):
- Header: short label, e.g. "Retry strategy"
- Question: the griller's exact question
- Options: 2-4 concrete choices derived from the plan and researcher findings,
  plus "Other / I'll explain"

Batch all decision prompts before continuing - do not interrupt synthesis
mid-way with more questions.

---

## Step 5: Report

Combine researcher findings and human decisions:

```
### Auto-Grill Report

**Solid** (Q3, Q7, Q9): codebase confirms these assumptions

**Human decisions made**
- Q2: [decision chosen + brief rationale]
- Q6: [decision chosen + brief rationale]

**Gaps to address**
- Q1: [question] — [specific contradiction or missing piece]
- Q5: [question] — [specific contradiction or missing piece]

**Verdict**: SOLID / NEEDS WORK / MAJOR GAPS
```

If gaps exist, ask: "Want to revise the plan and run another round?"

---

## Agent compatibility

| Feature | Claude Code | Codex / no subagents |
|---------|-------------|----------------------|
| Griller | Spawn separate Agent | Do the griller role yourself first: reason about the plan, produce numbered questions with tags, then continue |
| Researcher | Spawn Explore Agent | Search the codebase yourself for each [CODE]/[BOTH] question before writing answers |
| Human prompts | `AskUserQuestion` tool | Output each decision as a numbered list and wait for the user to reply before continuing |

The adversarial separation matters: even when running solo, do the griller
pass first (questions only, no peeking at code), then the researcher pass
(search and answer), then surface decisions. Do not interleave them.
