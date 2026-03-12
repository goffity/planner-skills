# Example Plan: Add OAuth2 Authentication

This is a complete worked example showing what `/planner` produces for a single-repo Express API project.

---

## User Request

```
/planner Add user authentication with Google and GitHub OAuth2
```

## Clarifying Questions (Step 1.5)

```
Before I plan this out, a few questions to make sure I scope it right:

1. Should this support email/password login too, or OAuth2 only?
2. Are there existing user records we need to link OAuth accounts to?
3. Do we need role-based access control (admin, user, etc.)?
4. Should sessions use JWT tokens or server-side sessions?
5. Is there a specific OAuth library preference (passport.js, etc.)?

Answer what you can — skip any that aren't relevant.
```

**User answers:** "OAuth2 only, no existing users, no RBAC needed, use JWT, passport.js is fine"

## Codebase Analysis (Step 3)

```
## Codebase Analysis

**Commit:** a1b2c3d
**Tech Stack:** Node.js, Express, TypeScript, Prisma, PostgreSQL
**Architecture:** MVC pattern, route → controller → service → repository
**Project Type:** single-repo
**Affected Areas:**
- `src/routes/` — add auth routes
- `src/middleware/` — add JWT verification middleware
- `src/services/` — add auth service
- `prisma/schema.prisma` — add User model
- `src/config/` — add OAuth config
**Existing Tests:** Jest, ~60% coverage, tests in `__tests__/` directories
```

## Tasks Breakdown (Step 4)

| ID   | Title                                  | Type    | Priority | Effort | Deps      |
|------|----------------------------------------|---------|----------|--------|-----------|
| T1   | Add OAuth2 authentication              | feature | high     | XL     | -         |
| T1.1 | Add User model and database migration  | feature | high     | M (3h) | -         |
| T1.2 | Configure OAuth2 providers             | feature | high     | M (3h) | -         |
| T1.3 | Implement auth service with JWT        | feature | high     | M (4h) | T1.1      |
| T1.4 | Add auth routes and callback handlers  | feature | high     | M (4h) | T1.2,T1.3 |
| T1.5 | Add JWT verification middleware        | feature | high     | S (2h) | T1.3      |
| T1.6 | Add protected route examples           | feature | low      | S (1h) | T1.5      |
| T1.7 | Unit + Integration tests for auth      | test    | high     | L (6h) | T1.1~T1.6 |

## Dependency Graph

```
T1.1 ──→ T1.3 ──→ T1.4
T1.2 ────────────↗
T1.3 ──→ T1.5 ──→ T1.6
T1.1~T1.6 ──→ T1.7 (tests)
```

## Execution Waves

| Wave | Tasks      | Can Run in Parallel          | Estimated Duration |
|------|------------|-----------------------------|--------------------|
| 1    | T1.1, T1.2 | Yes — no shared dependencies | 3h (longest task)  |
| 2    | T1.3       | Solo — depends on T1.1       | 4h                 |
| 3    | T1.4, T1.5 | Yes — independent after T1.3 | 4h (longest task)  |
| 4    | T1.6       | Solo — depends on T1.5       | 1h                 |
| 5    | T1.7       | Solo — depends on all        | 6h                 |

**Total sequential estimate:** 23h
**With parallelization:** 18h (saved 5h)

## Risks

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| OAuth provider API changes | medium | low | Pin passport strategy versions; add integration test against real OAuth flow |
| JWT secret leak | high | low | Use env vars, never commit secrets; rotate keys with zero-downtime |
| Token refresh race conditions | medium | medium | Use short-lived access tokens (15min) + refresh token rotation |

### Rollback Plan
1. Remove auth middleware from protected routes (instant rollback)
2. Revert Prisma migration: `npx prisma migrate resolve --rolled-back add-user-model`
3. Remove OAuth app credentials from provider dashboards

## Task Details

### T1: Add OAuth2 authentication

**Type:** feature | **Priority:** high | **Effort:** XL (23h)

**Current Flow:**
1. API has no authentication — all endpoints are public
2. No user concept exists in the database
3. Anyone can access any endpoint without credentials

**New Flow (after implementation):**
1. User visits `/auth/google` or `/auth/github` to start OAuth flow
2. Provider redirects to `/auth/callback/{provider}` after consent
3. System creates or finds user record, issues JWT token
4. User includes `Authorization: Bearer <token>` in subsequent requests
5. Protected routes verify JWT via middleware; public routes remain open

### T1.1: Add User model and database migration

**Type:** feature | **Priority:** high | **Effort:** M (3h)
**Status:** pending
**Log:**

**Description:** Create Prisma User model with OAuth fields (provider, providerId, email, name, avatar) and run migration.

**Affected Files:**
- `prisma/schema.prisma` — add User model
- `prisma/migrations/` — generated migration

**Acceptance Criteria:**
- [ ] User model has: id, email, name, avatar, provider, providerId, createdAt, updatedAt
- [ ] Migration runs successfully on fresh database
- [ ] Unique constraint on (provider, providerId) pair

### T1.2: Configure OAuth2 providers

**Type:** feature | **Priority:** high | **Effort:** M (3h)
**Status:** pending
**Log:**

**Description:** Set up passport.js with Google and GitHub strategies. Load client ID/secret from environment variables.

**Affected Files:**
- `src/config/passport.ts` — new file, strategy configuration
- `src/config/env.ts` — add OAuth env vars
- `.env.example` — add placeholder keys

**Acceptance Criteria:**
- [ ] Google OAuth strategy configured and working
- [ ] GitHub OAuth strategy configured and working
- [ ] All secrets loaded from environment variables
- [ ] `.env.example` updated with required keys

### T1.7: Unit + Integration tests for auth

**Type:** test | **Priority:** high | **Effort:** L (6h)
**Status:** pending
**Log:**
**Dependencies:** T1.1, T1.2, T1.3, T1.4, T1.5, T1.6

**Description:** Comprehensive test coverage for the auth system.

**Test Files:**
- `src/services/__tests__/auth.service.test.ts` — unit tests for JWT and user creation
- `src/middleware/__tests__/auth.middleware.test.ts` — unit tests for JWT verification
- `src/routes/__tests__/auth.routes.test.ts` — integration tests for OAuth flow

**Test Cases:**
- [ ] User creation from OAuth profile
- [ ] Duplicate OAuth login returns existing user
- [ ] JWT token generation and verification
- [ ] Expired token rejection
- [ ] Protected route blocks unauthenticated requests
- [ ] Protected route allows valid JWT
- [ ] OAuth callback handles provider errors gracefully

**Acceptance Criteria:**
- [ ] Coverage ≥ 80% for auth-related code
- [ ] All tests pass in CI

---

## GitHub Issues Created

```
## Plan Created

**Plan File:** docs/plans/add-oauth2-auth.md
**Parent Task:** T1 - Add OAuth2 authentication (#42)
**Subtasks:** 7 subtasks
**Issues:** 8 on GitHub

| ID   | Title                                 | Issue | Status  |
|------|---------------------------------------|-------|---------|
| T1   | Add OAuth2 authentication             | #42   | Created |
| T1.1 | Add User model and database migration | #43   | Created |
| T1.2 | Configure OAuth2 providers            | #44   | Created |
| T1.3 | Implement auth service with JWT       | #45   | Created |
| T1.4 | Add auth routes and callback handlers | #46   | Created |
| T1.5 | Add JWT verification middleware       | #47   | Created |
| T1.6 | Add protected route examples          | #48   | Created |
| T1.7 | Unit + Integration tests for auth     | #49   | Created |

**Next Steps:**
- Start working on T1.1 and T1.2 (can run in parallel)
- View the full plan at docs/plans/add-oauth2-auth.md
```
