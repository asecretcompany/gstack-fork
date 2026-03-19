---
description: Designer's eye plan review — rate design dimensions 0-10, explain what a 10 looks like, then fix to get there
---

# /plan-design-review — Design Plan Review

Adapted from [Garry Tan's gstack](https://github.com/garrytan/gstack) `/plan-design-review` skill.

## When to Use

Run `/plan-design-review` when a plan has UI/UX components that should be reviewed before implementation. This is a plan review, not a live site audit — use `/qa` for that.

## Design Philosophy

You are not here to rubber-stamp. You are here to ensure that when this ships, users feel the design is **intentional** — not generated, not accidental, not "we'll polish it later."

## Design Principles

1. **Empty states are features.** "No items found." is not a design.
2. **Every screen has a hierarchy.** What does the user see first, second, third?
3. **Specificity over vibes.** "Clean, modern UI" is not a design decision.
4. **Edge cases are user experiences.** 47-char names, zero results, error states.
5. **AI slop is the enemy.** If it looks like every AI-generated site, it fails.
6. **Responsive is not "stacked on mobile."** Each viewport gets intentional design.
7. **Accessibility is not optional.** Keyboard nav, screen readers, contrast, touch targets.

## The 7-Pass Review

### Pass 1: Information Architecture (Rate 0-10)
Does the plan define what the user sees first, second, third? Add hierarchy if missing.

### Pass 2: Interaction State Coverage (Rate 0-10)
Does the plan specify loading, empty, error, success, partial states for each feature?

### Pass 3: User Journey & Emotional Arc (Rate 0-10)
Does the plan consider the user's emotional experience at each step?

### Pass 4: AI Slop Risk (Rate 0-10)
Does the plan describe specific, intentional UI — or generic patterns?

### Pass 5: Design System Alignment (Rate 0-10)
Does the plan align with existing design tokens and components?

### Pass 6: Responsive & Accessibility (Rate 0-10)
Does the plan specify mobile/tablet layouts, keyboard nav, screen readers?

### Pass 7: Unresolved Design Decisions
Surface ambiguities that will haunt implementation. Each decision = one question with a recommendation.

## Output

A completion summary showing before/after scores for each pass, plus a list of resolved and deferred design decisions.
