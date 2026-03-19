---
description: Post-ship documentation update — sync all project docs to match what was just shipped
---

# /document-release — Post-Ship Documentation

Adapted from [Garry Tan's gstack](https://github.com/garrytan/gstack) `/document-release` skill.

## When to Use

Run `/document-release` after shipping code (PR merged or about to merge). Updates every documentation file to match what actually shipped.

## How It Works

### Step 0: Detect Base Branch

```bash
gh pr view --json baseRefName -q .baseRefName 2>/dev/null || \
gh repo view --json defaultBranchRef -q .defaultBranchRef.name 2>/dev/null || \
echo "main"
```

### Step 1: Pre-Flight & Diff Analysis

```bash
git diff <base>...HEAD --stat
git log <base>..HEAD --oneline
git diff <base>...HEAD --name-only
find . -maxdepth 2 -name "*.md" -not -path "./.git/*" -not -path "./node_modules/*" | sort
```

Classify changes: new features, changed behavior, removed functionality, infrastructure.

### Step 2: Per-File Documentation Audit

Read each doc file and cross-reference against the diff:

- **README.md** — Features, install steps, examples still valid?
- **ARCHITECTURE.md** — Diagrams and design decisions still accurate?
- **CONTRIBUTING.md** — Setup instructions, test commands still work?
- **CHANGELOG.md** — Entries cover all changes?
- **Any other .md files** — Cross-reference against diff

Classify each needed update as:
- **Auto-update** — Factual corrections clearly from the diff
- **Ask user** — Narrative changes, section removal, large rewrites

### Step 3: Apply Auto-Updates

Make clear factual updates directly. For each file modified, note what specifically changed.

**Never auto-update:** Introduction/positioning, philosophy, security model, or remove entire sections.

### Step 4: Ask About Risky Changes

For ambiguous or narrative changes, ask the user with a recommendation and options.

### Step 5: CHANGELOG Voice Polish

**CRITICAL — Never clobber existing CHANGELOG entries.**

Only polish wording within existing entries:
- Lead with what the user can now **do**, not implementation details
- "You can now..." not "Refactored the..."
- Flag entries that read like commit messages

### Step 6: Cross-Doc Consistency

Check that all docs agree on features, versions, and structure. Every doc should be discoverable from README.

### Step 7: Commit & Report

```bash
git add <modified-doc-files>
git commit -m "docs: update project documentation for vX.Y.Z"
git push
```

Output documentation health summary:
```
Documentation health:
  README.md       [Updated] (added new feature to skills table)
  ARCHITECTURE.md [Current] (no changes needed)
  CONTRIBUTING.md [Updated] (fixed test command)
  CHANGELOG.md    [Voice polished]
```

## Key Rules

- Read before editing
- Never clobber CHANGELOG
- Never bump VERSION without asking
- Be explicit about what changed
- Voice: friendly, user-forward, not obscure
