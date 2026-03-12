# Issue Templates

Every issue follows the same base structure. The type-specific sections below show what to customize for each type.

## Base Structure

All issues use these sections:

```
## Summary
[What this task does, in 1-2 sentences]

## Background
[Why this task exists — context that helps someone understand the motivation]

## Steps
- [ ] [Implementation step 1]
- [ ] [Implementation step 2]
- [ ] [Implementation step 3]

## Technical Details
- **Affected files:** [files/modules to change]
- **Approach:** [how to implement]

## Acceptance Criteria
- [ ] [Testable criterion 1]
- [ ] [Testable criterion 2]
- [ ] [Testable criterion 3]

## Dependencies
- [Issue refs or "None"]

## Definition of Done
- [ ] Code implemented and reviewed
- [ ] Tests pass
- [ ] Documentation updated (if applicable)
```

## Type-Specific Guidance

Each type adds or modifies sections from the base structure. Only the differences are listed below — everything else stays the same.

### feature

**Background** adds:
- **User Need:** [problem or need]
- **Business Value:** [value delivered]

**Technical Details** adds:
- **New files:** [files to create]
- **API changes:** [if applicable]

**Definition of Done** adds:
- Unit tests written and passing
- Integration tests (if applicable)

### bug

**Summary:** Describe the bug concisely.

**Background** replaces with:
- **Current Behavior:** [what happens now]
- **Expected Behavior:** [what should happen]
- **Reproduction Steps:** numbered list

**Steps** should include:
- Identify root cause
- Implement fix
- Add regression test
- Verify fix

**Technical Details** adds:
- **Root cause:** [if known]
- **Impact:** [severity/scope]

### epic

**Summary:** High-level objective.

**Background** adds:
- **Goal:** [main objective]
- **Scope:** [boundaries]
- **Business Value:** [strategic value]

**Steps** organized into phases:
- Phase 1: [name] with sub-steps
- Phase 2: [name] with sub-steps

**Technical Details** adds:
- **Architecture impact:** [changes to system design]
- **Risk assessment:** [known risks]

**Definition of Done** adds:
- All sub-tasks completed
- Stakeholder sign-off

### story

**Summary** uses the format:
> As a [role], I want [action], so that [benefit].

**Background** adds:
- **User Journey:** [relevant user flow]

**Technical Details** adds:
- **UX considerations:** [if applicable]

### chore

**Background** adds:
- **Reason:** [why this maintenance is needed]
- **Impact if skipped:** [consequences of not doing it]

**Technical Details** adds:
- **Risk:** low / medium / high

### refactor

**Background** replaces with:
- **Current State:** [problems with existing code]
- **Target State:** [desired code structure]
- **Motivation:** [why refactor now]

**Steps** should include:
- Verify no behavior changes after each step

**Technical Details** adds:
- **Pattern applied:** [design pattern]
- **Breaking changes:** [none or list]

**Acceptance Criteria** should always include:
- All existing tests pass (no behavior change)

## Notes

- For Jira: use plain text instead of markdown checkboxes where Jira doesn't render them
- Fill in concrete details from the codebase analysis — don't leave placeholders in final issues
- Reference real issue numbers in the Dependencies section after issues are created (Pass 2)
