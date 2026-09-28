# Skill Development and Submission Guide

This guide is for engineers who create and maintain Skills in the `rd-skills` repository.

The repository is the source of truth. Skills should be developed, validated, reviewed, and merged through Git before Team Skill Hub synchronizes them.

## Workflow

```text
Developer
  -> create/update Skill
  -> local validation
  -> add/update Evaluation
  -> feature branch
  -> Merge Request
  -> CODEOWNERS review + CI
  -> merge main
  -> Team Skill Hub sync
```

## Repository layout

Use one business-domain directory and one directory per Skill:

```text
rd-skills/
├── ota/
│   ├── EVALUATION.yaml
│   └── code-review/
│       ├── SKILL.md
│       ├── references/
│       └── examples/
├── templates/
├── schemas/
└── CODEOWNERS
```

The Skill entry file must be named `SKILL.md`. Put optional deep reference material in `references/` and reusable examples in `examples/`.

## Create a Skill

Start from the repository template:

```bash
mkdir -p ota/my-skill
cp templates/SKILL.template.md ota/my-skill/SKILL.md
```

Required metadata includes `schema_version`, `name`, `description`, and the required metadata fields defined by `schemas/skill.schema.json`.

Use a stable kebab-case name, a description that clearly states when the Skill should be selected, real user-facing keywords, the correct visibility, a clear owner, and explicit client compatibility.

New Skills should normally start with:

```yaml
maturity: experimental
```

Do not use an artificially high priority to compensate for weak descriptions or keywords.

## Write executable instructions

A Skill is an execution guide for an Agent, not a long-form knowledge article.

Prefer concrete rules:

```text
For every removable API candidate:
1. identify known callers;
2. identify compatibility impact;
3. estimate deletion risk as Low, Medium, or High;
4. state required validation before deletion.
```

Avoid vague instructions such as `review carefully`.

Recommended sections:

```markdown
# Purpose
# When to use
# Instructions
# Constraints
# Output expectations
# Resources
```

## References and examples

Use `references/` for detailed checklists, specifications, architecture rules, or troubleshooting guidance that should only be loaded when needed.

Use `examples/` for sanitized real-world inputs and expected output patterns.

Never commit credentials, tokens, private customer data, unsanitized logs, or content prohibited by company policy.

## Add Evaluation

Stable capabilities should have deterministic regression coverage in the domain `EVALUATION.yaml`.

Example:

```yaml
schema_version: 1
id: ota-core-regression
cases:
  - id: resolve-my-skill
    target: skill
    operation: resolve
    query: review OTA update code for redundant APIs
    client: codex
    expect:
      selected: ota-my-skill
```

Cover at least a representative resolve case. Add ambiguity/regression cases when a new Skill overlaps existing Skills.

## Validate locally

Recommended workspace:

```text
workspace/
├── team-skill-hub/
└── rd-skills/
```

Install Hub dependencies once:

```bash
cd team-skill-hub
npm ci
```

Validate the repository:

```bash
npm run validate:repo -- \
  --path ../rd-skills \
  --id rd-skills \
  --revision working-tree
```

Run evaluation:

```bash
npm run evaluate:repo -- \
  --path ../rd-skills \
  --id rd-skills \
  --revision working-tree
```

Run one suite:

```bash
npm run evaluate:repo -- \
  --path ../rd-skills \
  --id rd-skills \
  --suite ota-core-regression
```

Both validation and relevant Evaluation cases should pass before submission.

## Branch and commit

Do not develop directly on `main`.

```bash
git checkout main
git pull
git checkout -b skill/ota-my-skill
```

Then:

```bash
git add ota/my-skill ota/EVALUATION.yaml
git commit -m "feat: add OTA my skill"
git push -u origin skill/ota-my-skill
```

Use focused commits and keep each Merge Request scoped to one logical change.

## Merge Request content

Describe:

- what changed;
- why the Skill is needed;
- which queries should resolve to it;
- which queries should not;
- overlap with existing Skills;
- permission/visibility impact;
- validation and Evaluation results.

Recommended checklist:

```text
[ ] validate:repo passed
[ ] evaluation passed
[ ] regression cases added/updated
[ ] no credentials or sensitive customer data
```

## CODEOWNERS review

Repository ownership controls review responsibility. For example:

```text
/ota/** @ota-team
/templates/** @ai-platform-team
/schemas/** @ai-platform-team
```

Reviewers should check duplication, routing metadata, executable instructions, visibility, sensitive data, dependencies, and regression coverage.

## After merge

After merge to GitLab `main`:

```text
GitLab push
  -> webhook
  -> Team Skill Hub sync
  -> scan and validate
  -> create validated snapshot
  -> atomically activate revision
```

If a new revision is invalid, the Hub should retain the Last Known Good revision.

Developers should not manually modify production Hub state.

## Verify availability

Use MCP tools such as:

```text
list_skills
search_skills
resolve_skill
get_skill
```

Verify both positive and negative routing behavior, visibility, and client compatibility.

## Updating or removing Skills

Keep the existing `name` stable whenever possible. Update the semantic version when behavior changes materially.

Before removing a stable Skill, prefer:

```yaml
maturity: deprecated
```

Migrate dependencies, Agent bindings, Evaluation cases, and users before deletion.

## Pre-submit checklist

```text
[ ] SKILL.md is correctly named
[ ] schema_version = 1
[ ] name is unique and stable
[ ] description clearly defines the trigger
[ ] keywords reflect real user wording
[ ] visibility is correct
[ ] owner is correct
[ ] maturity and priority are appropriate
[ ] compatibility is correct
[ ] references/examples contain no sensitive information
[ ] validate:repo passes
[ ] Evaluation passes
[ ] regression coverage is updated
[ ] MR explains routing and risk
[ ] required CODEOWNERS review is requested
```
