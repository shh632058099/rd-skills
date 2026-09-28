# Example usage

User request:

```text
Review the current OTA implementation and find unnecessary APIs and overly long call chains.
For every proposed deletion, explain the risk.
```

Expected behavior:

1. Inspect the current repository.
2. Build a lightweight map of OTA module boundaries.
3. Identify concrete API and call-chain candidates.
4. Use the review checklist when needed.
5. Return findings with file/symbol evidence and risk.
