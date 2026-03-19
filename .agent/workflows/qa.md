---
description: QA testing — open a real browser, test like a user, find bugs, fix them, re-verify
---

# /qa — Browser QA + Bug Fix

Adapted from [Garry Tan's gstack](https://github.com/garrytan/gstack) `/qa` skill.

## When to Use

Run `/qa` when a feature is ready for testing, after a deploy to staging, or when you want to verify your app works end-to-end. This uses **Antigravity's native browser agent** — no custom binary needed.

## Tiers

- **Quick** — Critical + high severity only. 30-second smoke test.
- **Standard** (default) — + medium severity. 5-15 minute systematic exploration.
- **Exhaustive** — + low/cosmetic. Full coverage.

## How It Works

### Step 1: Setup

Parse the user's request for:
- **Target URL** — auto-detect or ask
- **Tier** — Quick, Standard, or Exhaustive
- **Scope** — Full app or focused on specific pages
- **Auth** — Any login credentials needed?

Check for a clean working tree:
```bash
git status --porcelain
```

If dirty, ask the user to commit or stash first (QA needs atomic commits per bug fix).

### Step 2: Detect Mode

**Diff-aware mode** (automatic when on a feature branch with no URL):
```bash
git diff main...HEAD --name-only
git log main..HEAD --oneline
```
Identify affected pages/routes from changed files and test those specifically.

**Full mode** (when URL is provided): Systematic exploration of the entire app.

### Step 3: Browser Testing

Use Antigravity's `browser_subagent` to test the app:

1. **Navigate** to target URL
2. **Take screenshots** for evidence
3. **Check console** for errors
4. **Test interactions** — click buttons, fill forms, submit
5. **Test states** — empty, loading, error, overflow
6. **Test responsive** — mobile and desktop viewports

For each page:
- Visual scan for layout issues
- Click every interactive element
- Fill and submit forms (test empty, invalid, edge cases)
- Check navigation paths in and out
- Check console for new JS errors after interactions

### Step 4: Document Issues

For each bug found:
1. Take a "before" screenshot
2. Perform the action
3. Take an "after" screenshot
4. Write repro steps

### Step 5: Fix Loop (for each fixable issue, in severity order)

1. **Locate source** — grep for the relevant code
2. **Fix** — minimal change that resolves the issue
3. **Commit** — `git commit -m "fix(qa): ISSUE-NNN — short description"`
4. **Re-test** — verify the fix in the browser
5. **Classify** — verified, best-effort, or reverted

### Step 6: Regression Tests

For each verified fix, write a regression test that:
- Fails without the fix
- Passes with the fix

### Step 7: Report

Output a structured QA report with:
- Health score (0-100, weighted across categories)
- Issues found with severity and evidence
- Fixes applied with before/after screenshots
- Ship readiness summary

## Health Score Categories

| Category | Weight |
|---|---|
| Console errors | 15% |
| Broken links | 10% |
| Visual issues | 10% |
| Functional bugs | 20% |
| UX problems | 15% |
| Performance | 10% |
| Content issues | 5% |
| Accessibility | 15% |

## Key Rules

1. **Repro is everything.** Every issue needs a screenshot.
2. **Never include credentials** in reports — use `[REDACTED]`.
3. **Test as a user**, not a developer — don't read source code during QA.
4. **One commit per fix.** Never bundle.
5. **Check console after every interaction.**
6. **Always use the browser.** Never substitute unit tests for browser QA.
