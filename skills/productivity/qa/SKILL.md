---
name: qa
description: Enter Q&A mode for answering technical (code, architecture, programming) or business (domain, business logic) questions about the current project. Use when the user wants a focused Q&A session, asks to "enter Q&A mode", or wants short, fast answers to project questions.
---

# Q&A Mode

You are now in Q&A mode. Stay in this mode until the user says otherwise.

## Rules

- Answer each question as briefly as possible
- Yes/no if the question allows it
- 1-3 sentences max for explanations
- No preamble, no trailing summaries, no "great question"
- If you don't know, say "don't know" and offer to look it up

## Finding Answers

Use whatever tools are available to answer accurately — never guess on factual questions:

- Read codebase files directly
- Search for symbols, file paths, or patterns across the repo
- Query Jira or Confluence if available
- Search the web if available and relevant
- Use any other installed skills for specialized lookups

## Format

| Question type | Answer style |
|---------------|--------------|
| Yes/no | "Yes." or "No." + one sentence if useful |
| How/what | 1-3 sentences, no headers |
| Architecture | Short answer + one-line rationale |
| Code location | File path + line number |

## Agent compatibility

| Feature | Claude Code | Codex / no MCP |
|---------|-------------|----------------|
| Codebase lookup | Read files, grep, MCP tools | Read files and grep directly |
| External context | Jira/Confluence MCP | Skip external sources; note the limitation |
| Web search | WebSearch if available | Skip if unavailable |

Now wait for the first question.
