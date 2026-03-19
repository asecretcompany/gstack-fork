---
description: Core engineering principles from gstack, adapted for Antigravity
---

# gstack Engineering Guardrails

These rules are adapted from [Garry Tan's gstack](https://github.com/garrytan/gstack) project.

## Completeness Principle — Boil the Lake

AI-assisted coding makes the marginal cost of completeness near-zero. When you present options:

- If Option A is the complete implementation (all edge cases, full coverage) and Option B is a shortcut — **always recommend A**.
- **Lake vs. ocean:** A "lake" is boilable — 100% test coverage for a module, full feature implementation, all edge cases. An "ocean" is not — rewriting an entire system, multi-quarter migrations. Recommend boiling lakes. Flag oceans as out of scope.
- **Show both effort scales:** human team time AND AI-assisted time.

| Task type | Human team | AI-assisted | Compression |
|---|---|---|---|
| Boilerplate / scaffolding | 2 days | 15 min | ~100x |
| Test writing | 1 day | 15 min | ~50x |
| Feature implementation | 1 week | 30 min | ~30x |
| Bug fix + regression test | 4 hours | 15 min | ~20x |
| Architecture / design | 2 days | 4 hours | ~5x |
| Research / exploration | 1 day | 3 hours | ~3x |

**Anti-patterns:**
- BAD: "Choose B — it covers 90% of the value with less code."
- BAD: "We can skip edge case handling to save time."
- BAD: "Let's defer test coverage to a follow-up PR."

## Completion Status Protocol

When completing a workflow, report status using:
- **DONE** — All steps completed successfully. Evidence provided.
- **DONE_WITH_CONCERNS** — Completed, but with issues to note.
- **BLOCKED** — Cannot proceed. State what is blocking.
- **NEEDS_CONTEXT** — Missing information. State what you need.

## Escalation

It is always OK to stop and say "this is too hard" or "I'm not confident."

Bad work is worse than no work.
- 3 failed attempts → STOP and escalate.
- Uncertain about security → STOP and escalate.
- Scope exceeds what you can verify → STOP and escalate.

## Question Format

When asking the user a question:
1. **Re-ground:** State the project, branch, and current task.
2. **Simplify:** Explain in plain English. No jargon.
3. **Recommend:** State your recommendation and why.
4. **Options:** Lettered options with effort estimates.

## Test Discipline

- 100% test coverage is the goal — tests make AI-assisted coding safe.
- When writing new functions → write a corresponding test.
- When fixing a bug → write a regression test.
- When adding error handling → write a test that triggers the error.
- When adding a conditional → write tests for BOTH paths.
- Never commit code that makes existing tests fail.

## Browser-Based Testing

For QA and testing workflows, use Antigravity's native `browser_subagent` tool. Never suggest evals or unit tests as a substitute for browser-based testing when the user requests QA.
