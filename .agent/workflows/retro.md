---
description: Weekly engineering retrospective — commit analysis, team metrics, shipping streaks, and actionable insights
---

# /retro — Weekly Engineering Retrospective

Adapted from [Garry Tan's gstack](https://github.com/garrytan/gstack) `/retro` skill.

## When to Use

Run `/retro` at the end of a work week or sprint. Analyzes commit history, work patterns, and code quality metrics with persistent trend tracking.

## Arguments

- `/retro` — last 7 days (default)
- `/retro 24h` — last 24 hours
- `/retro 14d` — last 14 days
- `/retro 30d` — last 30 days
- `/retro compare` — compare this period vs prior period

## How It Works

### Step 1: Gather Data

Fetch and identify the current user, then run data collection:

```bash
git fetch origin <default> --quiet
git config user.name
git config user.email
```

Collect (run in parallel — these are independent):
1. All commits with timestamps, subject, hash, author, files changed
2. Per-commit test vs production LOC breakdown
3. Commit timestamps for session detection
4. File hotspot analysis (most frequently changed files)
5. PR numbers from commit messages
6. Per-author file hotspots
7. Per-author commit counts
8. Test file count

### Step 2: Compute Metrics

| Metric | Value |
|---|---|
| Commits to main | N |
| Contributors | N |
| PRs merged | N |
| Total insertions / deletions | +N / -N |
| Net LOC added | N |
| Test LOC ratio | N% |
| Active days | N |
| Sessions detected | N |
| Avg LOC/session-hour | N |

### Step 3: Time Distribution

Show hourly commit histogram. Identify peak hours, dead zones, late-night clusters.

### Step 4: Session Detection

Detect work sessions using 45-minute gap threshold:
- **Deep sessions** (50+ min)
- **Medium sessions** (20-50 min)
- **Micro sessions** (<20 min)

### Step 5: Commit Type Breakdown

Categorize by conventional commit prefix (feat/fix/refactor/test/chore). Flag if fix ratio >50%.

### Step 6: Hotspot Analysis

Top 10 most-changed files. Flag churning files (changed 5+ times).

### Step 7: Focus Score & Ship of the Week

- **Focus score:** % of commits in the most-changed directory
- **Ship of the week:** Highest-LOC PR with context on why it matters

### Step 8: Team Analysis (if multiple contributors)

Per-person:
- Commits, LOC, test ratio
- Areas of focus
- Commit type mix
- Session patterns
- **Praise** — 1-2 specific things done well (anchored in commits)
- **Growth opportunity** — 1 constructive suggestion (framed as investment)

### Step 9: Streak Tracking

```bash
git log origin/<default> --format="%ad" --date=format:"%Y-%m-%d" | sort -u
```

Count consecutive days with commits.

### Step 10: Save & Compare

Save metrics to `.context/retros/` as JSON. Compare with prior retro if available.

## Output Structure

1. **Tweetable summary** (one line)
2. Summary table
3. Trends vs last retro (if available)
4. Time & session patterns
5. Shipping velocity
6. Code quality signals
7. Focus & highlights
8. Your personal deep-dive
9. Team breakdown (if applicable)
10. Top 3 wins
11. 3 things to improve
12. 3 habits for next week

## Tone

- Encouraging but candid
- Specific and concrete — anchored in actual commits
- Skip generic praise
- Frame improvements as leveling up, not criticism
- ~3000-4500 words
