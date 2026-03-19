---
description: Engineering-level plan review — lock architecture, data flow, edge cases, tests
---

# /plan-eng-review — Engineering Plan Review

Adapted from [Garry Tan's gstack](https://github.com/garrytan/gstack) `/plan-eng-review` skill.

## When to Use

Run `/plan-eng-review` after `/plan-ceo-review` or when you have a technically specified plan that needs architecture validation. This is the "lock the execution plan" step.

## How It Works

### Step 0: Detect Base Branch

```bash
gh pr view --json baseRefName -q .baseRefName 2>/dev/null || \
gh repo view --json defaultBranchRef -q .defaultBranchRef.name 2>/dev/null || \
echo "main"
```

### Step 1: Read Context

Read the design doc, plan, existing codebase structure, and any prior reviews. Cross-reference with `ARCHITECTURE.md` and test files if they exist.

### Step 2: Architecture Review

Generate or validate:
- **Data flow diagram** — ASCII diagram showing how data moves through the system
- **State machine** — If the feature has states, diagram them explicitly
- **Component boundaries** — What talks to what? Where are the interfaces?
- **Error paths** — What happens when things fail at each step?

### Step 3: Edge Case Analysis

For each feature in the plan:
- What happens with empty/null inputs?
- What happens at scale (1000x normal load)?
- What happens with concurrent access?
- What happens with network failures?
- What happens with malformed data?

### Step 4: Test Matrix

Generate a test matrix covering:

| Scenario | Type | Priority | Covered? |
|---|---|---|---|
| Happy path | Unit | P0 | ☐ |
| Empty input | Unit | P0 | ☐ |
| Error handling | Integration | P1 | ☐ |
| Concurrent access | Integration | P1 | ☐ |
| Performance bounds | Smoke | P2 | ☐ |

### Step 5: Security Review

Check for:
- Authentication and authorization gaps
- Input validation
- Data exposure risks
- Dependency vulnerabilities

### Step 6: Review Verdict

Rate each dimension 0-10 and fix anything below 8:
- Architecture clarity
- Error handling completeness
- Test coverage plan
- Security posture
- Performance considerations

Ask the user to approve the locked plan before implementation begins.

## Key Principles

- Force hidden assumptions into the open
- ASCII diagrams over prose for data flow
- Every conditional needs a test for both paths
- If the plan touches >5 files, flag the blast radius
