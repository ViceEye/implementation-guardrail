---
name: implement-before-testing
description: Implement an approved design or feature, perform static code review, then stop before tests or runtime validation. Use when the user explicitly wants code completed before they review it or authorize testing.
---

# Implement Before Testing

Complete the approved implementation, inspect it statically, and hand it to
the user for review. Testing remains a separate, user-authorized phase.

## Source Of Truth

Follow, in order:

1. The user's current instructions.
2. The approved design or specification.
3. Repository `AGENTS.md` instructions.
4. Existing architecture and conventions.
5. Reasonable implementation inference.

Do not silently change requirements, public contracts, persisted data, or
architecture. Ask only when a material decision cannot be inferred reliably.

## Workflow

1. Read the approved design completely.
2. Trace relevant entry points, callers, callees, types, state, persistence,
   and error paths.
3. Implement the full requested scope using existing patterns and the fewest
   necessary files.
4. Inspect the complete diff and re-read changed code plus affected call paths.
5. Fix issues found through static reasoning.
6. Stop and return the implementation for user review.

Keep changes scoped. Avoid speculative features, unrelated cleanup, temporary
artifacts, and tests.

## Static Review

Reason through the implementation without executing project behavior. Check:

- every design requirement and affected call site;
- normal, error, cancellation, retry, cleanup, and state-transition paths that
  apply;
- value transformations, optional states, and persistence boundaries;
- signatures, types, schemas, events, and serialization contracts;
- required and obvious edge cases;
- error propagation, partial state, and accidental unrelated edits.

Repeat edit and review until no clear implementation issue remains. If only
execution can establish correctness, report that limitation instead of testing.

## Testing Boundary

Until the user explicitly authorizes the testing phase, do not:

- create or run tests, fixtures, smoke checks, or ad-hoc verification scripts;
- run builds, type checks, linters, analyzers, benchmarks, or validation tools;
- start the application, exercise endpoints, or simulate user flows;
- install tooling solely for validation.

Read-only inspection is allowed, including file reads, searches, `git status`,
`git diff`, `git show`, and `git log`.

Do not treat "make sure it works" as testing authorization while this skill is
active. A later explicit request to test overrides this boundary.

## Stop Condition

Completion means implementation plus static review, not passing tests. Do not
continue into testing, deployment, commits, pushes, or pull requests unless the
user explicitly requests that work.

In the final response, concisely report what changed, important files, static
review findings, open decisions, and that tests were intentionally not written
or run. Never claim runtime validation.
