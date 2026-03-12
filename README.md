# Planner Skill

Task planning and breakdown skill for AI coding agents. Analyzes your codebase, breaks work into well-scoped subtasks with dependencies, writes a trackable plan file, and optionally creates GitHub or Jira issues.

## Install

```bash
# Global (all projects)
npx skills add goffity/planner-skill -g

# Project-level
npx skills add goffity/planner-skill
```

## Usage

```
/planner Add user authentication with OAuth2
/planner Refactor database layer to use repository pattern
/planner Fix payment timeout when users checkout with multiple items
/planner
```

## What It Does

1. **Analyzes your codebase** — tech stack, architecture, affected files, test setup
2. **Detects project structure** — auto-detects monorepo vs single-repo
3. **Breaks down tasks** — parent task + subtasks with dependencies, effort estimates, and acceptance criteria
4. **Creates a plan file** — saved to `docs/plans/<name>.md` for tracking
5. **Creates issues** — optionally pushes tasks to GitHub Issues or Jira

## Features

- **Auto monorepo detection** — detects pnpm workspaces, Lerna, Nx, Turborepo, multiple go.mod, etc. Uses `[service]: description` prefix for subtasks in monorepo, plain titles for single-repo
- **Multi-language output** — plan content in English or Thai
- **Dependency tracking** — subtasks ordered by dependencies with execution order
- **Current Flow / New Flow** — parent task always includes before/after behavior description
- **Test subtask** — always includes a test subtask as the last item
- **Two-pass issue creation** — creates all issues first, then links dependencies

## Plan File Example

Plans are saved to `docs/plans/` with this structure:

```markdown
# Plan: Add Authentication Layer

**Status:** approved
**Total Tasks:** 5
**Estimated Effort:** XL

| ID   | Title                          | Type    | Priority | Effort | Deps     |
|------|--------------------------------|---------|----------|--------|----------|
| T1   | Add authentication layer       | feature | high     | XL     | -        |
| T1.1 | Create auth middleware         | feature | high     | M      | -        |
| T1.2 | Add JWT token validation       | feature | high     | M      | T1.1     |
| T1.3 | Add login UI components        | feature | high     | M      | T1.2     |
| T1.4 | Unit + Integration tests       | test    | medium   | M      | T1.1~T1.3|
```

## Supported Agents

Works with any agent that supports the skills protocol, including:

Claude Code, Cursor, GitHub Copilot, Cline, Windsurf, Roo, and more.

## Supported Issue Trackers

- **GitHub Issues** — via `gh` CLI
- **Jira** — via jira-client script or `/jira` skill

## License

MIT
