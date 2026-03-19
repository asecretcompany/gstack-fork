---
description: CEO/founder-level plan review — rethink the problem, challenge scope, find the 10-star product
---

# /plan-ceo-review — CEO Plan Review

Adapted from [Garry Tan's gstack](https://github.com/garrytan/gstack) `/plan-ceo-review` skill.

## When to Use

Run `/plan-ceo-review` after `/office-hours` or when you have a plan/design doc that needs strategic review. This is the "rethink the product" step — not rubber-stamping, but actively challenging whether you're building the right thing.

## How It Works

### Step 0: Detect Base Branch

```bash
# Determine base branch for diff context
gh pr view --json baseRefName -q .baseRefName 2>/dev/null || \
gh repo view --json defaultBranchRef -q .defaultBranchRef.name 2>/dev/null || \
echo "main"
```

### Step 1: Read Context

Read the design document, plan, or PR description. Understand what's being proposed and why.

### Step 2: Choose Review Mode

Ask the user which mode to use:

- **Scope Expansion** — Find the 10-star product hiding inside the request. What could this become?
- **Selective Expansion** — Keep the core, but identify 2-3 high-leverage additions
- **Hold Scope** — Validate the current scope is right-sized and well-prioritized
- **Scope Reduction** — Cut ruthlessly. What's the absolute minimum that proves the hypothesis?

**RECOMMENDATION:** Choose Scope Reduction for first versions, Hold Scope for mature products.

### Step 3: The 10-Section Review

Run through each section, rating and improving:

1. **Problem clarity** — Is the pain real, specific, and urgent?
2. **User definition** — Are the first users clearly identified?
3. **Solution uniqueness** — Why this approach vs. alternatives?
4. **Scope appropriateness** — Right-sized for the current stage?
5. **Technical feasibility** — Can this actually be built as described?
6. **Risk identification** — What could go wrong? What's the contingency?
7. **Success metrics** — How will you know this worked?
8. **Architecture readiness** — Is the plan specific enough to implement?
9. **Test coverage plan** — How will correctness be verified?
10. **Security considerations** — Any auth, data, or privacy concerns?

### Step 4: Generate Recommendations

For each section scoring below 8/10:
- Explain the gap
- Propose a fix
- Ask the user to approve or modify

### Step 5: Update the Plan

Edit the design document with approved changes. The improved plan feeds into `/plan-eng-review`.

## Key Principles

- Challenge premises, not just details
- "You said X, but what you actually described is Y" is a valid review finding
- Always recommend the complete option over shortcuts (Boil the Lake)
- Show effort in both human and AI-assisted time
