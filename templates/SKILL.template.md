---
schema_version: 1
name: example-skill
version: 0.1.0
description: >
  Short description of when this skill should be used.

metadata:
  audience:
    - developer

  domain:
    - example

  category:
    - development

  keywords:
    - example

  visibility:
    - internal

  maturity: experimental
  owner: example-team
  priority: 50
  depends_on: []
  compatibility:
    codex: true
    claude_code: true
    customer_agent: false
---

# Purpose

Describe what this skill helps the agent accomplish.

# Instructions

1. Understand the task and repository context.
2. Apply this skill only when it matches the user's goal.
3. Prefer concrete evidence from the current repository.
4. Explain risks and uncertainty.

# Resources

Load files under `references/` only when needed.

