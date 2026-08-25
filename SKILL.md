---
name: implementation-guardrail
description: >
  Implement an already-approved design or feature without entering the testing
  or validation phase. Use when the user asks to start implementation, write
  the code first, implement according to an existing design/spec, or explicitly
  says tests should happen only after they review the implementation.
---

# Implementation Guardrail

## Invocation Modes

Default mode: **off**.

- No argument: apply the guardrail to the current request only. Do not keep it
  active for later turns.
- `on` or `auto`: keep the guardrail active for matching implementation work in
  this conversation until the user selects `off`.
- `off`: disable persistent guardrail mode and return to the normal workflow.
  Do not start deferred testing unless the user requests it.
- `status`: report the current mode without performing implementation work.

Treat `$implementation-guardrail on`, `$implementation-guardrail auto`,
`$implementation-guardrail off`, and `$implementation-guardrail status` as
mode commands.

Persistent modes apply only to the current conversation. A new conversation
uses the installed invocation policy. Do not infer persistence from a previous
one-shot invocation.

## Purpose

Implement an already-defined design completely while deliberately stopping
before the testing and validation phase.

The workflow for this skill is:

1. Understand the approved design.
2. Read the relevant existing code.
3. Implement the required changes.
4. Re-read the changed code and relevant surrounding code.
5. Inspect the diff and reason about the implementation statically.
6. Fix any implementation problems found through code inspection.
7. Stop and hand the implementation to the user for review.

Do NOT proceed into testing unless the user explicitly authorizes testing
after reviewing the implementation.

---

# Core Rule

This is an IMPLEMENTATION-ONLY phase.

The completion condition is:

> Implementation complete + static code reasoning complete.

The completion condition is NOT:

> Tests pass.

Never automatically transition from implementation into testing.

---

# Source of Truth

Follow this priority order:

1. The user's current instructions.
2. The approved design/specification provided by the user.
3. Repository-level `AGENTS.md` instructions.
4. Existing architecture and code conventions.
5. Reasonable implementation inference.

Do not silently change the approved architecture, requirements, data model,
API contract, or business behavior.

If implementation reveals a conflict between the design and the existing
codebase, investigate the surrounding code before deciding what to do.

---

# Confidence Rule

Do not interrupt the user for minor implementation details.

If an implementation detail can be determined confidently from:

- the approved design,
- surrounding code,
- existing patterns,
- type definitions,
- call sites,
- naming conventions,
- or established architecture,

make the reasonable implementation decision yourself.

Ask the user only when:

- an important product or business rule is ambiguous;
- two materially different implementations are both plausible;
- implementation would require changing the approved design;
- a public API, persisted data model, protocol, or architecture must change
  unexpectedly;
- or confidence in the correct decision is below 90%.

Do not invent important business rules just to avoid asking a question.

---

# Phase 1 — Understand the Design

Before modifying code:

1. Read the provided design/specification completely.
2. Identify:
   - required behavior;
   - affected components;
   - expected data flow;
   - interfaces or APIs involved;
   - state changes;
   - error handling expectations;
   - relevant edge cases;
   - compatibility constraints.
3. Determine which existing files and call paths are likely involved.

Do not create a new design when one already exists.

Do not expand scope beyond what is required for the approved design.

---

# Phase 2 — Inspect the Existing Code

Read enough existing code to understand the implementation context.

Inspect:

- relevant entry points;
- callers and callees;
- interfaces and type definitions;
- models and schemas;
- state ownership;
- persistence boundaries when relevant;
- error propagation;
- existing abstractions;
- neighboring implementations of similar behavior.

Prefer extending existing patterns over introducing unnecessary abstractions.

Do not modify code merely because it could be cleaner.

Avoid unrelated refactors.

---

# Phase 3 — Implement

Implement the approved design completely.

While editing:

- keep changes scoped to the requested feature;
- preserve existing architecture unless the design explicitly changes it;
- reuse existing abstractions where appropriate;
- maintain repository naming and style conventions;
- handle required edge cases in the implementation itself;
- update all necessary call sites when contracts change;
- keep data flow internally consistent;
- remove temporary implementation artifacts before finishing.

Do not add speculative features.

Do not add unrelated cleanup.

Do not create tests during this phase.

---

# Strict No-Testing Rule

Unless the user explicitly authorizes testing after reviewing the code,
DO NOT:

- write unit tests;
- write integration tests;
- write end-to-end tests;
- write acceptance tests;
- write regression tests;
- create temporary test scripts;
- create ad-hoc verification scripts;
- add test fixtures solely for validation;
- run existing tests;
- run newly written tests;
- run test suites;
- perform manual acceptance testing;
- launch the application to verify behavior;
- exercise endpoints to verify behavior;
- simulate user flows for verification.

Do not interpret "make sure it works" as permission to run tests when this
skill is active.

The user intentionally wants to review the implementation before testing.

---

# No Validation Commands

During this phase, validation must be based on reading and reasoning about
the code.

Unless explicitly requested by the user, do NOT run commands whose primary
purpose is validating the implementation, including:

- test runners;
- builds used as correctness checks;
- type-check commands used as correctness checks;
- linters used as correctness checks;
- static analyzers;
- validation scripts;
- application startup used for verification;
- benchmark commands;
- smoke tests.

Do not install or introduce tooling solely to validate the implementation.

Read-only repository inspection commands are allowed, for example:

- `git status`
- `git diff`
- `git diff --stat`
- `git show`
- `git log`
- `rg`
- `grep`
- `find`
- reading files directly

Commands necessary purely for inspecting repository state are not considered
testing.

If a command may execute project behavior rather than merely inspect files,
do not run it unless the user has explicitly authorized it.

---

# Phase 4 — Static Implementation Review

After implementation is complete, perform a code-reading review.

First inspect the complete diff.

Then re-read:

- every modified file;
- important surrounding code;
- callers affected by the changes;
- callees relied upon by the new logic;
- modified interfaces and their consumers.

Reason through the code without executing it.

Check specifically for:

## Design coverage

Confirm that every required part of the approved design has been implemented.

Look for:

- missing branches;
- forgotten call sites;
- partially implemented flows;
- TODOs accidentally left behind;
- requirements represented in the design but absent from code.

## Control flow

Reason through:

- normal path;
- error path;
- early returns;
- retries when applicable;
- cancellation when applicable;
- state transitions;
- asynchronous flows;
- cleanup behavior.

## Data flow

Trace important values from source to destination.

Confirm:

- transformations are correct;
- required fields are preserved;
- optional/null states are handled appropriately;
- values reach the correct layer;
- stale or unintended values cannot leak into the new flow.

## Interface consistency

Check:

- function signatures;
- method signatures;
- type definitions;
- API contracts;
- event payloads;
- schemas;
- configuration structures;
- serialization/deserialization boundaries.

Verify that callers and implementations remain consistent.

## Edge cases

Reason about edge cases required by the design and obvious cases implied by
the surrounding code.

Do not invent speculative edge cases unrelated to the requested behavior.

## Error handling

Confirm that:

- errors are handled at the correct layer;
- errors are not silently swallowed unintentionally;
- existing error semantics are preserved unless intentionally changed;
- partial state cannot be left inconsistent where the design requires
  atomicity or cleanup.

## Scope control

Check the diff for:

- accidental unrelated edits;
- unnecessary refactors;
- duplicate abstractions;
- debugging code;
- temporary comments;
- dead code introduced by the change.

---

# Phase 5 — Fix Findings

If static inspection reveals an implementation problem:

1. Fix the code directly.
2. Re-read the affected section.
3. Re-check the relevant call path.
4. Re-inspect the resulting diff.

Repeat until no clear implementation issue remains.

This is still static code review.

Do not convert discovered uncertainty into a test unless the user has
authorized the testing phase.

If the only remaining way to establish correctness is execution or testing,
report that fact to the user instead of performing the test.

---

# Stop Condition

After implementation and static review are complete, STOP.

Do not automatically continue with:

- testing;
- QA;
- build verification;
- acceptance verification;
- deployment;
- commits;
- pushes;
- pull requests;

unless explicitly requested by the user.

Testing is a separate phase controlled by the user.

---

# Final Response

Keep the final response concise.

Report:

1. **Implemented**
   - what behavior was implemented.

2. **Main files changed**
   - the important files modified and their purpose.

3. **Static logic review**
   - the important control flow, data flow, interfaces, edge cases, or error
     handling checked through code reading.

4. **Open decisions**
   - anything that still requires user review or a product/design decision.
   - omit this section if there are none.

5. **Testing**
   - explicitly state that tests were not written or run because testing is
     intentionally deferred until after user review.

Never claim:

- "tests pass";
- "verified working";
- "fully validated";
- or equivalent execution-based confidence

when no tests or runtime validation were performed.
