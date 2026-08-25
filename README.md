# Implementation Guardrail

A Codex skill that keeps implementation scoped and controlled: finish the
approved work, then wait for human review before testing or runtime validation.

## What It Does

- Implements an existing design or specification.
- Reviews the diff and affected call paths through static code reasoning.
- Stops before tests, builds, linters, application startup, or deployment.
- Resumes validation only after explicit user authorization.

This skill is useful when implementation needs review before test output or
automated fixes influence the next step. It is not a general instruction to
skip testing permanently.

## Install

Clone the repository into your Codex skills directory:

```powershell
git clone https://github.com/ViceEye/implementation-guardrail.git "$HOME/.codex/skills/implementation-guardrail"
```

Restart Codex after installation. The skill can be selected automatically from
matching requests or invoked explicitly as `$implementation-guardrail`.

## Example

```text
Use $implementation-guardrail to implement the approved pagination design.
Stop after static review so I can inspect the diff before tests run.
```

## License

[MIT](LICENSE)
