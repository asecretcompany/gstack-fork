---
description: Automated release workflow — sync main, run tests, audit coverage, push, open PR
---

# /ship — Automated Release

Adapted from [Garry Tan's gstack](https://github.com/garrytan/gstack) `/ship` skill.

## When to Use

Run `/ship` when you're ready to merge. This is the fully automated "release engineer" — it runs pre-flight checks, ensures tests pass, and creates the PR.

## How It Works

### Step 0: Detect Base Branch

```bash
gh pr view --json baseRefName -q .baseRefName 2>/dev/null || \
gh repo view --json defaultBranchRef -q .defaultBranchRef.name 2>/dev/null || \
echo "main"
```

### Step 1: Pre-Flight Checks

```bash
# Ensure we're NOT on the base branch
git branch --show-current

# Check for uncommitted changes
git status --porcelain

# Sync with remote
git fetch origin <base>
git merge origin/<base> --no-edit
```

**Stop conditions:**
- On the base branch → abort
- Uncommitted changes → ask: commit, stash, or abort
- Merge conflicts → abort and explain

### Step 2: Detect Test Framework

```bash
# Check for existing test infrastructure
ls jest.config.* vitest.config.* playwright.config.* .rspec pytest.ini pyproject.toml phpunit.xml 2>/dev/null
ls -d test/ tests/ spec/ __tests__/ cypress/ e2e/ 2>/dev/null
```

If no test framework exists, offer to bootstrap one (with user approval).

### Step 3: Run Tests

Run the project's test suite. If tests fail:
1. Analyze the failure
2. If it's a pre-existing failure (exists on base branch), note and continue
3. If it's a new failure, attempt to fix (max 2 attempts)
4. If fix fails, **STOP** — do not ship failing tests

### Step 4: Coverage Audit

Check test coverage for the changed files:
```bash
git diff <base>...HEAD --name-only
```

For each changed file, verify it has corresponding test coverage. Flag uncovered files.

### Step 5: Pre-Landing Review

Quick pass over the diff for:
- Debug prints / console.logs left behind
- TODO/FIXME without tracked issues
- Hardcoded credentials or secrets
- Large commented-out code blocks

Auto-fix obvious items. Ask about ambiguous ones.

### Step 6: Create PR

```bash
# Push the branch
git push origin HEAD

# Create PR (or update existing)
gh pr create --fill
```

If a PR already exists, update it with the latest changes.

### Step 7: Documentation Update

Optionally invoke `/document-release` to sync all project docs.

### Step 8: Report

```
SHIP REPORT
════════════════════════════════════════
Branch:          <branch-name>
Commits:         N commits
Tests:           X passed, Y failed (Z new)
Coverage:        N files covered / M total changed
Auto-fixes:      N applied
PR:              <url>
Status:          SHIPPED | BLOCKED
════════════════════════════════════════
```

## Key Principles

- Never ship failing tests
- One PR per feature branch
- All auto-fixes get their own commits
- Tests are the cheapest lake to boil — add them
- If anything is uncertain, **STOP** and ask
