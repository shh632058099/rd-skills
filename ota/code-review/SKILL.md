---
schema_version: 1
name: ota-code-review
version: 1.0.0
description: >
  Review an OTA implementation with focus on unnecessary APIs,
  excessive call chains, redundant abstractions, dead code,
  maintainability and deletion risk.

metadata:
  audience:
    - developer

  domain:
    - ota
    - embedded

  category:
    - code-review
    - refactor
    - architecture

  keywords:
    - ota
    - api
    - interface
    - code review
    - simplify
    - refactor
    - call chain
    - dead code
    - 接口
    - 删除
    - 精简
    - 调用链
    - 代码审查

  visibility:
    - internal

  maturity: stable
  owner: ota-team
  priority: 90
  depends_on: []
  compatibility:
    codex: true
    claude_code: true
    customer_agent: false
---

# OTA Code Review

Use this skill when reviewing an OTA implementation for simplification, maintainability, and unnecessary complexity.

## Review workflow

1. Understand the OTA module boundaries and the responsibility of each major component.
2. Identify public and internal APIs that appear redundant or unnecessary.
3. Trace call chains and identify chains that can be shortened without hiding required behavior.
4. Identify duplicated abstractions, wrappers, adapters, or state transitions.
5. Identify dead code and unreachable or obsolete compatibility paths.
6. For every deletion or simplification suggestion, explain the concrete risk.
7. Do not classify an API as removable only because it currently has few call sites.
8. Provide concrete file paths, symbols, call paths, and reasons whenever possible.
9. Classify change risk as Low, Medium, or High.
10. Prefer the smallest coherent change over broad redesign.

## Output expectations

For each finding provide:

- location
- current behavior
- proposed simplification
- expected benefit
- deletion/change risk
- dependencies or compatibility concerns
- recommended validation

## Additional reference

Load `references/review-checklist.md` when a more detailed review checklist is useful.

