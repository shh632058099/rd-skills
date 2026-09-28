# R&D Skills

Independent Git repository for shared engineering skills consumed by Team Skill Hub.

## Repository role

This repository is the source of truth for R&D skills.

It is intentionally separate from the `team-skill-hub` server repository.

Typical workflow:

```text
Developer
  ↓
Create / update Skill
  ↓
Pull Request / Merge Request
  ↓
Review + CI
  ↓
Merge main
  ↓
Team Skill Hub syncs latest revision
```

## Initial skill

```text
ota/code-review
```

Use this skill for OTA implementation reviews focused on:

- unnecessary APIs
- excessive call chains
- redundant abstractions
- dead code
- maintainability
- deletion risk

