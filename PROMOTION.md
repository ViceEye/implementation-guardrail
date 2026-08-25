# Promotion Copy

## English

### One-liner

Implementation Guardrail keeps coding agents focused: implement the approved
scope, review the diff statically, then stop before autonomous validation.

### Short Post

Coding agents often keep going after implementation: running tests,
self-correcting, widening scope, and quietly becoming their own acceptance
reviewer.

Implementation Guardrail adds a clean checkpoint to Codex:

- implement the approved design;
- inspect the diff and affected code paths;
- stop before tests, builds, or runtime validation;
- let the user decide when validation begins.

It does not ban testing. It separates implementation from validation so work
stays scoped, reviewable, and under human control.

Repository: https://github.com/ViceEye/implementation-guardrail

### Slogan

**Implement with focus. Validate by consent.**

## 中文

### 一句话介绍

Implementation Guardrail 为 Codex 实现流程设置护栏：完成约定范围并进行静态审查后停止，不让 Agent 自动进入测试和验收阶段。

### 短宣传文案

代码 Agent 常在“实现完成”后继续运行测试、自行修正、扩大改动范围，最后连验收也由自己完成。

Implementation Guardrail 为 Codex 增加一个清晰的人类检查点：

- 按已批准的设计完成实现；
- 静态检查 diff 和受影响调用链；
- 在测试、构建和运行时验证前停止；
- 由用户审查代码并决定何时进入验证阶段。

它不是“禁止测试”，而是把实现与验证拆成两个由用户控制的阶段，让改动更聚焦、更易审查，也避免 Agent 自主验收。

项目地址：https://github.com/ViceEye/implementation-guardrail

### Slogan

**专注实现，验证由你决定。**
