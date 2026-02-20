# TDD Workflow

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin that enforces strict Test-Driven Development through dedicated subagents for each phase of the Red-Green-Refactor cycle.

## How It Works

The plugin provides a `tdd-workflow` skill that orchestrates three specialized subagents, each responsible for one TDD phase:

| Phase | Agent | Responsibility |
|-------|-------|----------------|
| **RED** | `tdd-test-writer` | Writes a failing test that captures expected behavior, then verifies it fails for the right reason |
| **GREEN** | `tdd-implementer` | Writes the minimal code needed to make the failing test pass — nothing more |
| **REFACTOR** | `tdd-refactorer` | Evaluates the implementation for improvement opportunities while keeping all tests green |

Strict phase gates prevent moving forward until each phase's exit criteria are met:

- RED: test must fail for the expected reason
- GREEN: all tests must pass
- REFACTOR: all tests must still pass after changes (or confirm no refactoring needed)

For multiple features, each completes the full cycle before the next begins — no interleaving.

## Installation

### Option A: Clone and load as a local plugin

```bash
git clone https://github.com/yuvalkramer-rg/tdd-workflow.git
claude --plugin-dir ./tdd-workflow
```

### Option B: Add as a plugin marketplace

From within Claude Code:

```
/plugin marketplace add yuvalkramer-rg/tdd-workflow
/plugin install tdd-workflow@tdd-workflow
```

### Option C: Copy the skill into your project

Copy `skills/tdd-workflow/SKILL.md` into your project's `.claude/skills/tdd-workflow/SKILL.md`. The skill will be auto-discovered on the next Claude Code session.

## Usage

Invoke the skill in Claude Code:

```
/tdd-workflow
```

Or ask Claude to use TDD:

> "Implement user login validation using TDD"

The orchestrator will break your request into testable features and run Red-Green-Refactor for each one, reporting progress at every phase transition.

## When to Use

- Implementing new features with test coverage
- Adding functionality where you want tests to drive the design
- Any time you want disciplined Red-Green-Refactor cycles

## When Not to Use

- Bug fixes where the failing test already exists
- Documentation, configuration, or non-code changes
- Exploratory prototyping with unclear requirements

## Project Structure

```
├── skills/
│   └── tdd-workflow/
│       └── SKILL.md          # Orchestrator skill
└── agents/
    ├── tdd-test-writer.md    # RED phase agent
    ├── tdd-implementer.md    # GREEN phase agent
    └── tdd-refactorer.md     # REFACTOR phase agent
```

## License

MIT
