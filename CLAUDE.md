# Project Instructions

<!-- Core agent-skills: spec-driven-development, test-driven-development, code-review-and-quality -->

---

# Spec-Driven Development

## Overview

Write a structured specification before writing any code. The spec is the shared source of truth between you and the human engineer — it defines what we're building, why, and how we'll know it's done. Code without a spec is guessing.

## When to Use

- Starting a new project or feature
- Requirements are ambiguous or incomplete
- The change touches multiple files or modules
- You're about to make an architectural decision
- The task would take more than 30 minutes to implement

**When NOT to use:** Single-line fixes, typo corrections, or changes where requirements are unambiguous and self-contained.

## The Gated Workflow

```
SPECIFY ──→ PLAN ──→ TASKS ──→ IMPLEMENT
   │          │        │          │
   ▼          ▼        ▼          ▼
 Human      Human    Human      Human
 reviews    reviews  reviews    reviews
```

### Phase 1: Specify

Surface assumptions immediately before writing any spec content:

```
ASSUMPTIONS I'M MAKING:
1. [assumption]
2. [assumption]
→ Correct me now or I'll proceed with these.
```

Write a spec covering six core areas:

1. **Objective** — What are we building and why?
2. **Commands** — Full executable commands (build, test, lint, dev)
3. **Project Structure** — Where source code, tests, and docs belong
4. **Code Style** — One real code snippet showing conventions
5. **Testing Strategy** — Framework, locations, coverage expectations
6. **Boundaries** — Always do / Ask first / Never do

### Phase 2: Plan

Identify major components, implementation order, risks, and verification checkpoints.

### Phase 3: Tasks

Each task:
- Completable in a single focused session
- Has explicit acceptance criteria
- Includes a verification step
- Touches no more than ~5 files

### Phase 4: Implement

Execute tasks one at a time. Follow the TDD cycle below.

## Red Flags

- Starting to write code without any written requirements
- Making architectural decisions without documenting them
- Implementing features not mentioned in any spec or task list

## Verification

- [ ] Spec covers all six core areas
- [ ] Human has reviewed and approved the spec
- [ ] Success criteria are specific and testable
- [ ] Spec is saved to a file in the repository

---

# Test-Driven Development

## Overview

Write a failing test before writing the code that makes it pass. For bug fixes, reproduce the bug with a test before attempting a fix. Tests are proof — "seems right" is not done.

## When to Use

- Implementing any new logic or behavior
- Fixing any bug (the Prove-It Pattern)
- Modifying existing functionality
- Any change that could break existing behavior

**When NOT to use:** Pure configuration changes, documentation updates, or static content changes with no behavioral impact.

## The TDD Cycle

```
    RED                GREEN              REFACTOR
 Write a test    Write minimal code    Clean up the
 that fails  ──→  to make it pass  ──→  implementation  ──→  (repeat)
```

### Step 1: RED — Write a Failing Test

Write the test first. It must fail. A test that passes immediately proves nothing.

### Step 2: GREEN — Make It Pass

Write the minimum code to make the test pass. Don't over-engineer.

### Step 3: REFACTOR — Clean Up

With tests green, improve the code without changing behavior. Run tests after every refactor step.

## The Prove-It Pattern (Bug Fixes)

```
Bug report arrives
       │
       ▼
  Write a test that demonstrates the bug
       │
       ▼
  Test FAILS (confirming the bug exists)
       │
       ▼
  Implement the fix
       │
       ▼
  Test PASSES (proving the fix works)
       │
       ▼
  Run full test suite (no regressions)
```

## Writing Good Tests

- **Test state, not interactions** — assert on outcomes, not which methods were called
- **DAMP over DRY** — each test tells a complete story without tracing through helpers
- **Prefer real implementations** over mocks; mock only at boundaries (external APIs, email)
- **Arrange-Act-Assert** pattern for structure
- **One assertion per concept**
- **Descriptive names** — reads like a specification

## Red Flags

- Writing code without any corresponding tests
- Tests that pass on the first run
- Bug fixes without reproduction tests
- Skipping tests to make the suite pass

## Verification

- [ ] Every new behavior has a corresponding test
- [ ] All tests pass
- [ ] Bug fixes include a reproduction test that failed before the fix
- [ ] No tests were skipped or disabled

---

# Code Review and Quality

## Overview

Multi-dimensional code review with quality gates. Every change gets reviewed before merge — no exceptions. Review covers five axes: correctness, readability, architecture, security, and performance.

## When to Use

- Before merging any PR or change
- After completing a feature implementation
- After any bug fix

## The Five-Axis Review

### 1. Correctness
- Matches spec/task requirements?
- Edge cases handled (null, empty, boundary values)?
- Error paths handled?
- Tests actually testing the right things?

### 2. Readability & Simplicity
- Names descriptive and consistent?
- Control flow straightforward?
- Could this be done in fewer lines?
- Are abstractions earning their complexity?

### 3. Architecture
- Follows existing patterns?
- Clean module boundaries?
- No circular dependencies?

### 4. Security
- User input validated and sanitized?
- Secrets kept out of code and logs?
- SQL queries parameterized?
- Outputs encoded to prevent XSS?
- External data treated as untrusted?

### 5. Performance
- Any N+1 query patterns?
- Any unbounded loops?
- Any unnecessary re-renders?
- Pagination on list endpoints?

## Review Comment Labels

| Prefix | Meaning |
|--------|---------|
| *(no prefix)* | Required — must fix before merge |
| **Critical:** | Blocks merge — security, data loss, broken functionality |
| **Nit:** | Minor, optional |
| **Consider:** | Suggestion, not required |
| **FYI** | Informational only |

## Red Flags

- PRs merged without any review
- "LGTM" without evidence of actual review
- Security-sensitive changes without security review
- No regression tests with bug fixes

## Verification

- [ ] All Critical issues resolved
- [ ] Tests pass
- [ ] Build succeeds
- [ ] Verification story documented
