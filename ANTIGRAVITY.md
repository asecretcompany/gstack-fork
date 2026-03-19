# gstack → Antigravity Port

> **Original project by [Garry Tan](https://x.com/garrytan) — MIT License**
> This fork adapts gstack for [Antigravity](https://blog.google/technology/google-deepmind/introducing-antigravity/), the agent-first IDE by Google DeepMind. Full credit to Garry and the gstack contributors for the original project and philosophy.

## What This Fork Does

gstack was built for Anthropic's Claude Code — a terminal-based AI coding assistant. This fork translates gstack's concepts into **native Antigravity primitives**, so you get the same structured engineering workflow without needing Claude Code at all.

| gstack (Claude Code) | Antigravity equivalent |
|---|---|
| `.claude/skills/` with `SKILL.md` | `.agent/workflows/` with workflow `.md` files |
| `CLAUDE.md` project context | `.agent/rules/` with rule `.md` files |
| Custom Playwright `browse` binary | Native `browser_subagent` tool (built-in) |
| `AskUserQuestion` tool | `notify_user` tool |
| Slash commands (`/plan-ceo-review`) | Slash commands (`/plan-ceo-review`) |
| Bash-based preambles | Not needed — Antigravity handles session state |

## Quick Start

### New project (no existing `.agent/` directory)

```bash
# Clone and copy the Antigravity files into your project
git clone https://github.com/asecretcompany/gstack-fork.git
cp -r gstack-fork/.agent /path/to/your/project/
```

### Existing project (already has `.agent/workflows/` or `.agent/rules/`)

The gstack workflow files have unique names (`plan-ceo-review.md`, `ship.md`, etc.) so they won't collide with your existing workflows. **Merge, don't overwrite:**

```bash
git clone https://github.com/asecretcompany/gstack-fork.git

# Copy workflows — won't overwrite existing files with the same name
cp -n gstack-fork/.agent/workflows/*.md /path/to/your/project/.agent/workflows/

# Copy rules — same, skip if file already exists
cp -n gstack-fork/.agent/rules/*.md /path/to/your/project/.agent/rules/
```

> **If you want to overwrite** (e.g., updating to a newer version of gstack workflows), drop the `-n` flag. Your own workflows with different filenames will be untouched either way.

### That's it

The `.agent/workflows/` and `.agent/rules/` directories are all Antigravity needs. Open your project in Antigravity and the workflows are available immediately as slash commands.

## Using the Workflows

Each gstack skill has been converted to an Antigravity workflow. Use them the same way — as slash commands in your Antigravity conversation:

### The Sprint: Think → Plan → Build → Review → Test → Ship → Reflect

| Workflow | What it does |
|---|---|
| `/office-hours` | Brainstorm and validate product ideas with six forcing questions |
| `/plan-ceo-review` | CEO/founder-level review — rethink the problem, find the 10-star product |
| `/plan-eng-review` | Engineering review — lock architecture, data flow, edge cases, tests |
| `/plan-design-review` | Designer's eye review — rate design dimensions 0-10, fix to 10 |
| `/review` | Pre-landing code review — find bugs that pass CI but break in prod |
| `/qa` | Full QA cycle — test in a real browser, find bugs, fix them, re-verify |
| `/investigate` | Systematic debugging with root cause investigation (no fixes without RCA) |
| `/ship` | Merge, test, and create PR — fully automated release workflow |
| `/retro` | Weekly engineering retrospective with team metrics and trends |
| `/document-release` | Post-ship documentation update — sync all docs to match what shipped |

### Usage Examples

```
You:    I want to build a notification system for my app.
You:    /office-hours
Agent:  [asks about the actual problem, challenges premises, generates approaches]
        [writes design doc]

You:    /plan-ceo-review
Agent:  [reads design doc, challenges scope, runs 10-section review]

You:    /plan-eng-review
Agent:  [ASCII diagrams, data flow, test matrix, failure modes]

You:    Approve. Build it.
Agent:  [implements the feature]

You:    /review
Agent:  [auto-fixes 2 issues, asks about a race condition]

You:    /qa https://staging.myapp.com
Agent:  [opens browser, clicks through flows, finds and fixes a bug]

You:    /ship
Agent:  Tests: 42 → 51 (+9 new). PR opened.
```

## Key Differences from Original gstack

### Browser: Native vs. Custom Binary

**Original gstack** ships a custom Playwright-based `browse` binary (`~100ms per command`) with commands like `$B goto`, `$B snapshot`, `$B click`.

**Antigravity** has a built-in `browser_subagent` tool that handles all browser interactions natively. No setup, no binary, no dependencies. The QA and browse workflows in this fork use Antigravity's native browser instead.

### Session Management: Simplified

**Original gstack** uses bash preambles for session tracking, update checks, analytics, and contributor mode.

**Antigravity** handles most of this natively — no preambles needed. The workflows in this fork strip out the Claude Code-specific session machinery and focus on the actual workflow logic.

### Rules vs. CLAUDE.md

**Original gstack** uses `CLAUDE.md` for project-level instructions that persist across conversations.

**Antigravity** uses `.agent/rules/` for the same purpose. This fork includes `gstack-guardrails.md` in `.agent/rules/` with the core gstack principles adapted for Antigravity.

## Gemini CLI: An Alternative Path

If you prefer a terminal-based workflow similar to the original Claude Code experience, Google's [Gemini CLI](https://github.com/google-gemini/gemini-cli) is the closest equivalent:

```bash
# Install Gemini CLI
npm install -g @anthropic-ai/gemini-cli

# Use gstack skills from the terminal
gemini-cli --workspace /path/to/your/project
```

Gemini CLI reads the same `.agent/` directory structure, so your workflows and rules work in both Antigravity (IDE) and Gemini CLI (terminal).

## Philosophy: Boil the Lake

gstack follows the **Boil the Lake** principle — when AI makes the marginal cost of completeness near-zero, always do the complete thing. Don't ship 80% when 100% costs minutes more.

This fork preserves that philosophy. Every workflow pushes for completeness:
- Tests for every fix
- Reviews before every ship
- Design critique before every build
- Documentation updates after every release

> "The delta between 80 lines and 150 lines is meaningless with AI. 'Good enough' is the wrong instinct when 'complete' costs minutes more." — gstack

## File Structure

```
.agent/
├── workflows/                    # Antigravity workflows (slash commands)
│   ├── office-hours.md           # /office-hours — brainstorm & validate
│   ├── plan-ceo-review.md        # /plan-ceo-review — CEO-level review
│   ├── plan-eng-review.md        # /plan-eng-review — engineering review
│   ├── plan-design-review.md     # /plan-design-review — design review
│   ├── review.md                 # /review — pre-landing code review
│   ├── qa.md                     # /qa — browser QA + bug fix
│   ├── investigate.md            # /investigate — root cause debugging
│   ├── ship.md                   # /ship — automated release
│   ├── retro.md                  # /retro — weekly retrospective
│   └── document-release.md       # /document-release — post-ship docs
└── rules/
    └── gstack-guardrails.md      # Persistent project rules
```

## Credits

- **[Garry Tan](https://github.com/garrytan)** — Original gstack creator, philosophy, and all skill designs
- **[gstack](https://github.com/garrytan/gstack)** — The upstream project (MIT License)
- **[A Secret Company](https://asecretcompany.com)** — This Antigravity port

## License

MIT — Same as the original gstack. Free forever. Go build something.
