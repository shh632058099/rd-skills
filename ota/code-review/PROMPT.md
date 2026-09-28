---
schema_version: 1
name: ota-code-review-prompt
version: 1.0.0
description: Reusable prompt for reviewing OTA implementations for unnecessary APIs, long call chains, redundant abstractions, and deletion risk.
metadata:
  audience: [developer]
  visibility: [internal]
  keywords: [ota, code-review, api, simplify, refactor, call-chain, 接口, 精简, 调用链]
  category: [code-review, refactor]
  owner: ota-team
  compatibility:
    codex: true
    claude_code: true
    customer_agent: false
---

# OTA Code Review Prompt

Review the current OTA implementation with an emphasis on simplification and maintainability.

For every recommendation:

1. identify the exact file, symbol, interface, or call path;
2. explain why it is unnecessary, redundant, or overly indirect;
3. describe the proposed simplification;
4. estimate deletion/change risk as Low, Medium, or High;
5. call out compatibility, state-management, error-handling, and test-coverage risks;
6. avoid deleting an interface only because it currently has few references.

Prioritize:

- removable public/private APIs;
- excessively long call chains;
- duplicate wrapper or abstraction layers;
- dead or unreachable code;
- duplicated state ownership;
- redundant error translation;
- opportunities to shorten control flow without changing externally visible behavior.

Prefer the smallest safe change.
