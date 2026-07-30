# Evaluation Prompts

Use these scenarios to test source discovery, proportional depth, evidence discipline, focused updates, and output structure.

## 1. RFC

- **Prompt:** `Give me a mental model of docs/example-rfc.md.`
- **Mode:** `RFC_MODE`
- **Depth:** Based on the RFC's semantic complexity; usually medium or large.
- **Required:** Meeting card, purpose/scope, proposal flow, decisions, risks, unknowns, evidence map, rehearsal.
**Watch for:** Section-by-section summarisation, invented decisions, lost source terminology, or no distinction between proposal and accepted decision.

## 2. Pull request

- **Prompt:** `Create a mental model for PR #123.`
- **Mode:** `PR_MODE`
- **Depth:** Based on the change, not PR prose or file count alone.
- **Required:** Meeting card, semantic spine, before/after behaviour, state/failure/verification models, change map, coverage, rehearsal.
**Watch for:** Trusting the description, omitting the diff, hiding uninspected files, or treating passing checks as proof.

## 3. Pull-request review

- **Prompt:** `Review PR #123 and create its mental model.`
- **Mode:** `PR_MODE + REVIEW_MODE`
- **Depth:** Match semantic complexity and risk.
- **Required:** All PR essentials plus evidence-backed findings grouped by severity, or an explicit statement that none were found.
**Watch for:** Reviewing before understanding, speculative findings, unsupported severity, or approving for the user.

## 4. Current changes

- **Prompt:** `Give me a mental model of the current branch changes.`
- **Mode:** `CODE_CHANGE_MODE`
- **Depth:** Small to large based on the branch diff.
- **Required:** Git-derived scope, change classification, semantic spine, before/after behaviour, changed tests, verification, coverage.
**Watch for:** Ignoring uncommitted work, describing filenames without tracing behaviour, or claiming tests ran when they did not.

## 5. System data flow

- **Prompt:** `Explain how stock updates reach the database.`
- **Mode:** `SYSTEM_MODE`
- **Depth:** Usually medium because it crosses execution and persistence boundaries.
- **Required:** Bounded question, entry point, real call/data path, state owner, transaction/failure model, tests, evidence, rehearsal.
**Watch for:** Describing the whole repository, substituting intended architecture for implementation, or inventing queue semantics.

## 6. Small bug fix

- **Prompt:** `Give me a mental model of this three-file bug fix.`
- **Mode:** `CODE_CHANGE_MODE`
- **Depth:** Small unless the files cross high-risk boundaries.
- **Required:** Meeting card, concise Layer 1, relevant evidence/change map, rehearsal.
**Watch for:** Filling every template section, excessive diagrams, raw file-count assumptions, or bloated prose.

## 7. Large refactor

- **Prompt:** `Create a mental model for this 70-file refactoring PR.`
- **Mode:** `PR_MODE` with the large-PR extension.
- **Depth:** Large.
- **Required:** Complete change manifest, classifications, semantic spine, before/after behaviour, high-risk boundaries, deep/sample/excluded counts, reviewability assessment, coverage.
**Watch for:** Reading alphabetically, conflating mechanical and behavioural changes, no sampling disclosure, or equating 70 files with 70 semantic changes.

## 8. Attention constraint

- **Prompt:** `Explain this subsystem, but keep Layer 0 and Layer 1 brief.`
- **Mode:** `SYSTEM_MODE`
- **Depth:** Source-dependent, with aggressively compressed upper layers.
- **Required:** One-screen meeting card, five-minute Layer 1, relevant collapsible Layer 2, evidence, rehearsal.
**Watch for:** Moving all essential information below the fold, repeating details, or exceeding the meeting-card limits.

## 9. Incremental update

- **Prompt:** `Update the existing mental model after this PR.`
- **Mode:** Existing source mode plus `PR_MODE`.
- **Depth:** Only affected sections unless the PR invalidates the model broadly.
- **Required:** Existing artifact read first, new evidence, affected-section updates, change record, recalculated confidence.
**Watch for:** Regenerating unrelated prose, discarding useful content, failing to explain changes, or retaining stale confidence.

## 10. Meeting card only

- **Prompt:** `Give me only the meeting card for this RFC.`
- **Mode:** `RFC_MODE` with focused output.
- **Depth:** Source analysis sufficient to support a roughly 120-word card; output only Layer 0.
- **Required:** Seven fields or fewer, evidence-grounded wording, primary risk/decision.
**Watch for:** Emitting the full document, introductory prose, unsupported compression, or more than one screen.

## Additional focused-behaviour probes

- `Update the failure model.` should preserve unrelated sections and recalculate confidence if evidence changes.
- `Add a sequence diagram.` should add one only when ordering clarifies real interactions.
- `Quiz me using the mental model.` should use existing evidence and hide answers.
- `Compare this RFC with the current implementation.` should distinguish intended design from observed code.
- `Explain only the database and transaction model.` should omit unrelated template sections.
- `Show me the semantic spine of this PR.` should return the minimal end-to-end behavioural path and its evidence.
