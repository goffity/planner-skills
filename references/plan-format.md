# Plan File Format

Format for `docs/plans/<plan-name>.md` files created by `/planner`. Write the content in the language the user selected in Step 2.

## Naming Convention

- Use lowercase kebab-case: `add-user-auth.md`, `refactor-db-layer.md`
- Drop unnecessary words (a, an, the)
- Max 40 characters

## Template

```markdown
# Plan: [Plan Name]

**Created:** [YYYY-MM-DD HH:MM]
**Status:** draft | approved | in-progress | completed
**Author:** [git user.name]
**Total Tasks:** [N]
**Estimated Effort:** [sum of all task efforts]

## Context

[Context and goals for this plan]

## Codebase Analysis

**Commit:** [git short hash at time of analysis]
**Tech Stack:** [languages, frameworks, tools]
**Architecture:** [patterns, structure]
**Project Type:** single-repo | monorepo
**Affected Areas:**
- [file/module 1] - [reason]
- [file/module 2] - [reason]

## Tasks Breakdown

Tasks with multiple subtasks on the same topic are grouped as parent + subtask (T1, T1.1, T1.2, ...).
Tasks that don't need sub-breakdown use flat IDs (T2, T3, ...).

For monorepo/multi-repo projects, subtask titles use `[service]: description` prefix.
For single-repo projects, subtask titles use plain descriptions.

| ID   | Title | Type | Priority | Effort | Dependencies | Issue | Status |
|------|-------|------|----------|--------|--------------|-------|--------|
| T1   | [parent title] | feature | high | L | - | - | pending |
| T1.1 | [subtask title] | feature | high | M | - | - | pending |
| T1.2 | [subtask title] | feature | high | M | T1.1 | - | pending |
| T1.3 | Unit + Integration tests for [feature] | test | high | M | T1.1, T1.2 | - | pending |
| T2   | [flat task title] | chore | medium | S | T1 | - | pending |

## Task Details

### T1: [Parent Title]

**Type:** feature | **Priority:** high | **Effort:** L (sum of subtasks) | **Issue:** -

**Description:**
[Overview of this task group]

**Current Flow:**
1. [Current step-by-step user/system journey before implementation]
2. [Highlight limitations or pain points of current flow]
3. [...]

**New Flow (after implementation):**
1. [Step-by-step journey after implementation including new capabilities]
2. [Highlight changed behavior + backward compatible behavior]
3. [...]

**Subtasks Execution Order:**
1. T1.1 - [title] (no dependencies)
2. T1.2 - [title] (after T1.1)
3. T1.3 - [title] (after T1.1, T1.2)

---

#### T1.1: [Subtask Title]

**Type:** feature | **Priority:** high | **Effort:** M | **Issue:** -
**Status:** pending
**Log:**

**Description:**
[Subtask details]

**Affected Files:**
- `[specific/file/path]` — [what to change]
- `[specific/file/path]` — [what to change]

**Acceptance Criteria:**
- [ ] [criterion 1]
- [ ] [criterion 2]

---

#### T1.2: [Subtask Title]

**Type:** feature | **Priority:** high | **Effort:** M | **Issue:** -
**Dependencies:** T1.1

**Description:**
[Subtask details]

**Affected Files:**
- `[specific/file/path]` — [what to change]

**Acceptance Criteria:**
- [ ] [criterion 1]
- [ ] [criterion 2]

---

#### T1.N: Unit + Integration tests for [feature name]

**Type:** test | **Priority:** high | **Effort:** M | **Issue:** -
**Dependencies:** T1.1, T1.2, ... (all other subtasks)

**Description:**
[Tests covering all affected areas]

**Test Files:**
- `[path/to/service_test.go]` — [test scenarios]
- `[path/to/component.test.tsx]` — [test scenarios]

**Test Cases:**
- [ ] [test case 1]
- [ ] [test case 2]

**Acceptance Criteria:**
- [ ] Coverage ≥ 80%
- [ ] [other criteria]

---

### T2: [Flat Task Title]

**Type:** chore | **Priority:** medium | **Effort:** S | **Issue:** -
**Dependencies:** T1

**Description:**
[Task details]

**Affected Files:**
- [file path]

**Acceptance Criteria:**
- [ ] [criterion 1]
- [ ] [criterion 2]

---

[repeat for each task/subtask]

## Execution Order

Recommended execution order based on dependencies:

1. **T1** - [parent title]
   1. **T1.1** - [subtask title] (no dependencies)
   2. **T1.2** - [subtask title] (after T1.1)
   3. **T1.3** - [subtask title] (after T1.1, T1.2)
2. **T2** - [title] (after T1)

## Notes

- [Additional notes or caveats]
```

## Field Definitions

### Status Values

| Status | Meaning |
|--------|---------|
| `draft` | Just created, not yet reviewed |
| `approved` | User approved |
| `in-progress` | Work is in progress |
| `completed` | All tasks done |

### Task Status Values

| Status | Meaning |
|--------|---------|
| `pending` | Not started |
| `in-progress` | Currently being worked on |
| `done` | Completed |
| `blocked` | Waiting on dependency or external factor |
| `skipped` | Decided not to implement |

### Log Format

Each task can have a log of timestamped entries recording progress:

```
**Log:**
- [2026-03-13 10:30] Started — setting up database schema
- [2026-03-13 11:45] Done — 3 migration files created, tested locally
```

Logs are appended during implementation, not during planning. The plan file starts with empty logs.

### Effort Sizes

| Size | Time Estimate | Description |
|------|---------------|-------------|
| S | < 2 hours | Small task, minor changes |
| M | 2-4 hours | Medium task |
| L | 4-8 hours | Large task, requires planning |
| XL | > 8 hours | Very large, consider breaking down |

### Priority Levels

| Priority | When to Use |
|----------|-------------|
| high | Must do first, blocking others, core functionality |
| medium | Important but not urgent |
| low | Nice-to-have, can do later |

## Updating Plan File

After creating issues, update:
- Issue column in Tasks Breakdown table with issue reference (#123 or PROJ-123)
- Status from `draft` to `approved`

After starting work:
- Update task status to `in-progress` / `completed`
- Update plan status to `in-progress`
