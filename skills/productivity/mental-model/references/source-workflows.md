# Source Workflows

Load only the sections matching the inferred source mode. Combine a source workflow with Review mode only when the user explicitly asks for review.

## RFC mode

1. Read the complete RFC or design document when feasible. For a very large source, first map headings, then process coherent sections while maintaining one evidence ledger.
2. Preserve its terminology and consolidate across sections rather than producing disconnected summaries.
3. Identify the problem, context, goals, non-goals, actors, constraints, design, alternatives, decisions, dependencies, rollout, risks, and unresolved questions.
4. Separate accepted decisions, proposals, rejected alternatives, and unresolved options.
5. Map referenced epics or tickets to capabilities, dependencies, rollout stages, or risks only when evidence supports the relationship. Headings alone do not define ticket boundaries.

## PR mode

1. Determine the current repository. Use available GitHub tools or `gh` to retrieve the title, description, author, base/head branches, commits, changed files, statistics, reviews, relevant comments, checks, and full diff.
2. If remote access is unavailable, inspect a corresponding local branch or diff. Ask for the diff or access only when neither exists.
3. Treat PR prose as a claim. Inspect the shape of the actual change: modules, commits, schema, contracts, configuration, infrastructure, generated files, and tests.
4. Group files by semantic purpose: behavioural core, tests, database/schema, API/contracts, configuration/infrastructure, mechanical refactoring, and generated output.
5. Trace the semantic spine before peripheral changes: entry point, orchestration, domain decision, persistence or external effect, and verification test.
6. Separate intended behavioural change, intended unchanged behaviour, and mechanical movement or renaming.
7. Inspect individual commits when their structure conveys intent.
8. Report coverage, sampling, exclusions, and blind spots. Never imply every path was reviewed unless it was.

### Large PR extension

Use this extension when the PR has roughly more than 30 files, mixes behaviour with broad refactoring, crosses several boundaries, combines schema/API/queue/deployment/application work, is obscured by generated code, or lacks a quickly identifiable semantic spine.

1. Build and classify a complete change manifest before line-by-line review.
2. Separate generated and purely mechanical changes.
3. Find the smallest file set carrying the principal behaviour and trace one main path end to end.
4. Compare before and after behaviour.
5. Analyse high-risk boundaries first: transactions, migrations, concurrency, retries, idempotency, compatibility, authorization, integrations, deployment, and rollback.
6. State whether the PR is reasonably reviewable. Recommend a split or commit restructuring when independent semantic changes are entangled, and explain why.
7. Report total files; deeply inspected, sampled, mechanical/generated, and uninspected files; and affected areas.
8. Do not treat passing checks as proof of correctness or approve the PR for the user.

## Code-change mode

1. Inspect Git status, diff statistics, changed paths, relevant commits, changed tests, and the actual diff.
2. Classify the change as behavioural, corrective, refactoring, mechanical, configuration, migration, or mixed.
3. Trace the changed execution path and compare before with after.
4. Identify state, contracts, side effects, transaction boundaries, and external effects.
5. Run focused tests or static checks when permitted and useful. Record only commands actually run and their observed results.

## System mode

1. Anchor the investigation on the user's exact question.
2. Locate likely entry points, public interfaces, service/domain layers, data models, jobs/consumers/events/queues, integrations, configuration/deployment, and tests.
3. Follow real call and data paths, expanding the map only as evidence requires.
4. Do not describe the entire repository unless requested. State what was and was not investigated.
5. Separate current implementation from intended architecture documented elsewhere.

## Review mode

1. Build the source mental model before evaluating it.
2. Evaluate supported concerns across correctness, edge cases, maintainability, API/data design, performance, concurrency, transactions, retries/idempotency, security, observability, errors, tests, migrations, deployment, and rollback.
3. Classify findings as `Blocker`, `Important`, `Improvement`, or `Nit`.
4. For every finding provide evidence, problem, consequence, recommended action, and confidence. Do not manufacture findings to fill categories.
5. Separate findings from unanswered questions. State explicitly when no supported findings were identified.
