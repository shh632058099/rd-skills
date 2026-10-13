---
schema_version: 1
name: skill-creator
version: 0.1.0
description: >
  Create reusable engineering Skills from existing source code, documents,
  troubleshooting notes, or project experience, produce a Team Skill Hub compatible
  SKILL.md, and submit it as a Skill Candidate for administrator review.

metadata:
  audience:
    - developer

  domain:
    - meta
    - engineering-productivity

  category:
    - skill-authoring
    - knowledge-distillation
    - documentation

  keywords:
    - skill
    - create skill
    - distill
    - extract skill
    - code to skill
    - document to skill
    - submit skill
    - 创建 skill
    - 创建技能
    - 提炼 skill
    - 提炼技能
    - 代码转 skill
    - 文档转 skill
    - 提交 skill
    - 提交技能

  visibility:
    - internal

  maturity: experimental
  owner: ai-platform-team
  priority: 90
  depends_on: []
  compatibility:
    codex: true
    claude_code: true
    customer_agent: false
---

# Skill Creator

Use this skill when a developer wants to turn existing code, design documents,
runbooks, troubleshooting records, or repeated engineering experience into a
reusable team Skill.

## Goal

Produce a focused `SKILL.md` that teaches an agent how to perform a repeatable
engineering task. Do not merely summarize the source material. Extract the
decision process, execution steps, constraints, failure handling, and expected
output that another developer or agent can reuse.

## Creation workflow

1. Inspect the source material requested by the developer. Prefer direct
   evidence from the current repository and documents over assumptions.
2. Identify one reusable task. If the source contains several unrelated tasks,
   propose separate Skills rather than one broad "万能 Skill".
3. Extract:
   - trigger conditions: when the Skill should be selected;
   - prerequisites and required context;
   - deterministic execution steps;
   - important branches and decision criteria;
   - constraints, safety boundaries, and compatibility limitations;
   - validation steps and expected success signals;
   - common failure modes and recovery actions.
4. Remove project-local secrets, credentials, personal paths, temporary IDs,
   machine-specific values, and one-off incident details unless they are
   essential and can be safely parameterized.
5. Generate a complete `SKILL.md` using schema version 1. Include useful
   `audience`, `domain`, `category`, `keywords`, `visibility`, `maturity`,
   `owner`, `priority`, and compatibility metadata.
6. Put long background material in `references/` and representative reusable
   input/output examples in `examples/` instead of bloating `SKILL.md`.
7. Validate that the Skill is operational: another agent should be able to act
   from it without needing the original conversation.

## Skill collection

Choose one collection before submission:

- `rd-skills`: reusable team engineering capability that should be shared and
  maintained as an official R&D Skill.
- `custom-skills`: narrower developer/team customization that is useful to keep
  in Team Skill Hub but is not yet an official shared R&D capability.

When uncertain, default to `custom-skills` and explain why. Promote it to
`rd-skills` only when the procedure is broadly reusable and sufficiently stable.

## Suggested path

For `rd-skills`, prefer a domain-oriented path:

`<domain>/<skill-name>/SKILL.md`

For meta capabilities use:

`meta/<skill-name>/SKILL.md`

For `custom-skills`, prefer:

`custom/<skill-name>/SKILL.md`

Do not overwrite an existing Skill unless the developer explicitly intends to
update it.

## Submission behavior

Creating the files locally does **not** publish the Skill.

When the developer explicitly says `提交 skill`, `提交技能`, `submit skill`, or
otherwise clearly asks to submit the generated Skill for team review:

1. Re-read the final generated `SKILL.md`.
2. Confirm there are no secrets or machine-local values.
3. Determine `skill_collection` (`rd-skills` or `custom-skills`) and the target
   `suggested_path`.
4. Call the Team Skill Hub MCP tool `submit_skill_candidate` with the complete
   `SKILL.md` content. If that tool is unavailable, fall back to
   `submit_knowledge_candidate` with `suggested_type=skill`.
5. Never push directly to the shared Skill repository as part of this command.
   Submission creates a pending Candidate for administrator review.
6. Return the Candidate ID, collection, target repository/path, and current
   review status to the developer.

## Quality checklist

Before submission verify all of the following:

- `schema_version: 1` is present.
- `name` is stable, unique-looking, and kebab-case.
- `description` explains both trigger and purpose.
- keywords include likely Chinese and English user wording.
- instructions describe actions and decisions, not only background knowledge.
- source-specific secrets and temporary values are removed.
- validation and failure handling are included when applicable.
- the proposed collection and path are explicit.

## Resources

Load `references/submission-workflow.md` when deciding how a generated Skill
enters Team Skill Hub review and publication.

