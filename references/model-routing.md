# Model Routing Guide

## Model Profiles

### Haiku (claude-haiku-4-5)
Fast, cheap, lightweight. Use for tasks requiring minimal reasoning.

**Route to haiku when:**
- Single-file edits with clear, mechanical changes (typos, renames, formatting)
- Simple questions with obvious answers ("what does this function return?")
- Boilerplate/scaffolding generation from clear specs
- Running commands, listing files, reading configs
- Git operations (status, log, simple commits)
- Straightforward find-and-replace refactors
- Adding imports, exports, or simple wiring
- Writing simple tests for pure functions
- Translating clearly-specified content

**Signals:** short prompt, single file, no ambiguity, pattern-matching task, no architectural judgment needed.

### Sonnet (claude-sonnet-4-6)
Balanced speed and capability. The default workhorse.

**Route to sonnet when:**
- Multi-file changes following established patterns
- Debugging with clear error messages and stack traces
- Writing tests that require understanding component interactions
- Code review and identifying issues
- Implementing features from detailed specs
- Moderate refactoring (extracting functions, reorganizing modules)
- Research tasks (exploring codebase, understanding architecture)
- Writing documentation from existing code
- API integrations following documented patterns
- Database queries and migrations

**Signals:** moderate complexity, 2-5 files, some judgment needed but patterns exist, debugging with good signals.

### Opus (claude-opus-4-6)
Maximum capability. Use for tasks requiring deep reasoning.

**Route to opus when:**
- Architectural decisions (choosing patterns, designing systems)
- Complex debugging with no clear error signals
- Large-scale refactoring across many files with design decisions
- Novel problem-solving with no existing patterns to follow
- Security audits and vulnerability analysis
- Performance optimization requiring systemic understanding
- Implementing complex algorithms or data structures
- Cross-cutting concerns (auth systems, error handling frameworks)
- Planning multi-step implementations with tradeoffs
- Resolving subtle bugs involving race conditions, state management
- Tasks requiring long-range context and reasoning chains

**Signals:** high ambiguity, 5+ files, architectural judgment, novel territory, subtle/nuanced reasoning, no clear pattern to follow.

## Decision Shortcuts

| Signal | Model |
|--------|-------|
| "fix typo" / "rename" / "format" | haiku |
| Clear error message + stack trace | sonnet |
| "why is this slow/broken" (no clear cause) | opus |
| "add X" with clear spec | sonnet |
| "design/architect X" | opus |
| Single file, mechanical change | haiku |
| Multi-file, pattern-based | sonnet |
| Multi-file, requires design | opus |
| "explain this code" | sonnet |
| "review for security issues" | opus |
| Generate boilerplate | haiku |
| Write tests for complex interactions | sonnet |
| Debug flaky/intermittent test | opus |

## Cost Context
- Haiku: ~60x cheaper than Opus per token
- Sonnet: ~5x cheaper than Opus per token
- When in doubt between two tiers, prefer the lower tier — escalate only when the task genuinely demands deeper reasoning
