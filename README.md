# Implementation Guardrail

> **Implement with focus. Validate by consent.**

A Codex workflow guardrail that separates implementation from validation,
keeping agent work scoped, reviewable, and under user control.

## The Problem

Coding agents often keep going after the requested implementation is complete:
they run tests, interpret the results, make follow-up changes, expand the scope,
and effectively perform their own acceptance review.

Implementation Guardrail creates a deliberate human checkpoint. The agent
finishes the approved change, performs a static code review, reports what it
changed, and stops. Testing and runtime validation begin only when the user
explicitly authorizes the next phase.

## What It Does

- Implements an existing design or specification.
- Reviews the diff and affected call paths through static code reasoning.
- Stops before tests, builds, linters, application startup, or deployment.
- Resumes validation only after explicit user authorization.

This skill is useful when implementation needs review before test output or
automated fixes influence the next step. It is not a general instruction to
skip testing permanently.

## Before and After

| Without the guardrail | With the guardrail |
| --- | --- |
| Implement, test, self-correct, and keep expanding | Implement the approved scope and review it statically |
| Agent decides when the work is accepted | User reviews the diff and controls acceptance |
| Implementation and validation blur together | Implementation and validation stay separate |
| Test output can trigger unrequested changes | No runtime feedback loop starts without consent |

## Install

### One-Line Install

Install globally for Codex with the Agent Skills CLI:

```powershell
npx skills add ViceEye/implementation-guardrail --skill implementation-guardrail -g -a codex -y
```

No global package install is required. Restart Codex after installation.

Update or remove it later:

```powershell
npx skills update implementation-guardrail -g -y
npx skills remove implementation-guardrail -g -a codex -y
```

### Codex Plugin

The repository also ships as a skill-only Codex plugin and local marketplace:

```powershell
git clone https://github.com/ViceEye/implementation-guardrail.git
codex plugin marketplace add ./implementation-guardrail
codex plugin add implementation-guardrail@viceeye
```

Start a new Codex conversation after installation so the plugin is loaded.

### Manual Install

Alternatively, clone directly into the Codex skills directory:

```powershell
git clone https://github.com/ViceEye/implementation-guardrail.git "$HOME/.codex/skills/implementation-guardrail"
```

The skill is explicit-only by default.

## In-Codex Modes

Switch behavior directly in a Codex conversation:

```text
$implementation-guardrail           # Apply to this request only
$implementation-guardrail on        # Stay active for this conversation
$implementation-guardrail auto      # Alias for on
$implementation-guardrail off       # Disable persistent mode
$implementation-guardrail status    # Show the current conversation mode
```

`on` and `auto` last only for the current conversation. `off` does not start
deferred tests; request validation separately when ready.

## Usage

Implement an approved design and stop before validation:

```text
Use $implementation-guardrail to implement the approved pagination design.
Stop after static review so I can inspect the diff before tests run.
```

Apply it to an existing task without repeating the full policy:

```text
Use $implementation-guardrail. Implement the API changes in docs/design.md,
then hand the diff back to me without running tests or builds.
```

After reviewing the implementation, explicitly start the next phase:

```text
I reviewed the diff. You may now run the focused tests for this change.
```

## Workflow

```text
Approved design -> scoped implementation -> static review -> STOP
                                                        |
                                             user authorizes validation
                                                        |
                                               tests / build / QA
```

## Principles

- Prevent scope drift.
- Stop autonomous acceptance.
- Preserve human review checkpoints.
- Separate implementation from validation.

## License

[MIT](LICENSE)
