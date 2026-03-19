---
description: Systematic debugging with root cause investigation. No fixes without root cause.
---

# /investigate — Root Cause Debugging

Adapted from [Garry Tan's gstack](https://github.com/garrytan/gstack) `/investigate` skill.

## When to Use

Run `/investigate` when something is broken and you need to find out why. Use when asked to "debug this", "fix this bug", "why is this broken", or "root cause analysis".

## Iron Law

**NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST.**

Fixing symptoms creates whack-a-mole debugging. Every fix that doesn't address root cause makes the next bug harder to find.

## Phase 1: Root Cause Investigation

1. **Collect symptoms** — Read error messages, stack traces, reproduction steps. If not enough context, ask ONE question at a time.

2. **Read the code** — Trace the code path from symptom to potential causes. Grep for all references.

3. **Check recent changes:**
   ```bash
   git log --oneline -20 -- <affected-files>
   ```
   Was this working before? What changed?

4. **Reproduce** — Can you trigger the bug deterministically?

Output: **"Root cause hypothesis: ..."** — a specific, testable claim.

## Phase 2: Pattern Analysis

Check if the bug matches a known pattern:

| Pattern | Signature | Where to look |
|---|---|---|
| Race condition | Intermittent, timing-dependent | Concurrent shared state |
| Nil/null propagation | NoMethodError, TypeError | Missing guards on optionals |
| State corruption | Inconsistent data | Transactions, callbacks |
| Integration failure | Timeout, unexpected response | API calls, service boundaries |
| Configuration drift | Works locally, fails in staging | Env vars, feature flags |
| Stale cache | Shows old data | Redis, CDN, browser cache |

## Phase 3: Hypothesis Testing

Before writing ANY fix:

1. **Confirm the hypothesis** — Add a temporary log statement or assertion. Run reproduction. Does evidence match?

2. **If wrong** — Return to Phase 1. Gather more evidence. Do not guess.

3. **3-strike rule** — If 3 hypotheses fail, **STOP**. Ask:
   - A) Continue investigating with a new hypothesis
   - B) Escalate for human review
   - C) Add logging and catch it next time

**Red flags:**
- "Quick fix for now" → there is no "for now"
- Proposing a fix before tracing data flow → you're guessing
- Each fix reveals a new problem → wrong layer

## Phase 4: Implementation

Once root cause is confirmed:

1. **Fix the root cause**, not the symptom
2. **Minimal diff** — fewest files and lines changed
3. **Write a regression test** that fails without the fix and passes with it
4. **Run full test suite** — no regressions allowed
5. **If fix touches >5 files** — flag the blast radius

## Phase 5: Verification & Report

```
DEBUG REPORT
════════════════════════════════════════
Symptom:         [what the user observed]
Root cause:      [what was actually wrong]
Fix:             [what was changed]
Evidence:        [test output, repro showing fix works]
Regression test: [file:line of the new test]
Status:          DONE | DONE_WITH_CONCERNS | BLOCKED
════════════════════════════════════════
```

## Key Rules

- 3+ failed fix attempts → STOP and question the architecture
- Never apply a fix you cannot verify
- Never say "this should fix it" — verify and prove it
- If fix touches >5 files → ask about blast radius before proceeding
