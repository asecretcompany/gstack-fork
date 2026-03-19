---
description: Pre-landing code review — find bugs that pass CI but break in production. Auto-fix obvious issues.
---

# /review — Pre-Landing Code Review

Adapted from [Garry Tan's gstack](https://github.com/garrytan/gstack) `/review` skill.

## When to Use

Run `/review` before merging any branch. This is the "staff engineer reviews your PR" step — finding the bugs that pass CI but blow up in production.

## How It Works

### Step 0: Detect Base Branch

```bash
gh pr view --json baseRefName -q .baseRefName 2>/dev/null || \
gh repo view --json defaultBranchRef -q .defaultBranchRef.name 2>/dev/null || \
echo "main"
```

### Step 1: Gather Diff Context

```bash
git diff <base>...HEAD --stat
git diff <base>...HEAD --name-only
git log <base>..HEAD --oneline
```

### Step 2: Read Changed Files

Read every file that was changed. Understand the full context, not just the diff.

### Step 3: Review Checklist

For each changed file, check:

**Correctness:**
- [ ] Logic errors, off-by-one, wrong comparisons
- [ ] Null/nil handling — every optional value accessed safely
- [ ] Error paths — what happens when this fails?
- [ ] Race conditions — shared state accessed concurrently?

**Completeness:**
- [ ] Edge cases handled (empty, overflow, boundary)
- [ ] All code paths have test coverage
- [ ] Error messages are actionable for users

**Security:**
- [ ] No secrets, keys, or credentials in code
- [ ] Input validation on all user inputs
- [ ] Auth checks on protected resources

**Quality:**
- [ ] No dead code or debug prints left behind
- [ ] No TODO/FIXME without a tracked issue
- [ ] Naming is clear and consistent

### Step 4: Classify and Act

For each finding:
- **AUTO-FIX** — Obvious issues with a clear fix. Fix it, commit, note what was done.
- **ASK** — Ambiguous issues where the user needs to decide. Present options with a recommendation.
- **FLAG** — Informational findings that don't need action but should be noted.

### Step 5: Commit Auto-Fixes

```bash
git add <fixed-files>
git commit -m "fix(review): <description>"
```

One commit per fix. Never bundle.

### Step 6: Report

Output a structured review report:
- Total findings by category
- Auto-fixes applied (with details)
- Questions for user (with recommendations)
- Flags for awareness
- Ship readiness verdict: **SHIP** / **FIX FIRST** / **RETHINK**

## Key Principles

- Read the full file, not just the diff — context matters
- Fix the obvious things without asking
- Every fix gets its own atomic commit
- Never say "looks good" without evidence
