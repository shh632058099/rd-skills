# Skill submission workflow

The Skill Creator creates content; Team Skill Hub owns governance.

```text
existing code / docs / experience
            ↓
       skill-creator
            ↓
       generated SKILL.md
            ↓
 developer: "提交 skill"
            ↓
   submit_skill_candidate
            ↓
 Team Skill Hub pending review
            ↓
 administrator approve / reject
            ↓
 approved Candidate creates PR/MR
            ↓
 merge + repository synchronization
            ↓
       published Skill
```

## Collections

### rd-skills

Use for official reusable engineering capabilities. The administrator reviews
quality, metadata, reuse value, target path, and compatibility before allowing
publication.

### custom-skills

Use for developer/team-specific capabilities that should still be managed by
Team Skill Hub. They remain reviewable and auditable, and can later be promoted
to `rd-skills` without changing the creation and submission workflow.

## Important boundary

The developer-facing agent should not bypass Team Skill Hub by directly pushing
the generated Skill to a shared branch. The review Candidate is the handoff
between content generation and repository governance.

