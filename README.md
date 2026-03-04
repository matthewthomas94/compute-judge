# compute-judge

An intelligent model router skill for Claude Code that automatically selects the optimal Claude model for your task.

## What is compute-judge?

compute-judge is a Claude Code skill that intelligently routes tasks to the best-suited Claude model based on task complexity. Instead of manually choosing between models, you describe your task and compute-judge automatically dispatches it to:

- **Haiku** for simple, straightforward work
- **Sonnet** for moderate, multi-step tasks
- **Opus** for complex reasoning and architectural decisions

This optimizes both speed and token spend by avoiding over-provisioning simple tasks while ensuring complex work gets the reasoning power it needs.

## How it Works

1. **Invoke** — Run `/compute-judge <task description>`
2. **Classify** — compute-judge analyzes your task across four dimensions:
   - **Ambiguity** — Is the task clearly specified?
   - **Scope** — How many files/systems are involved?
   - **Reasoning depth** — Does it need pattern matching or novel judgment?
   - **Risk** — Could a mistake cause harm?
3. **Route** — Task is dispatched to the optimal model via a subagent
4. **Execute** — The selected model completes your task end-to-end

## Model Tiers

### Haiku (Fast & Cheap)
Use for straightforward, mechanical tasks:
- Single-file edits (typos, renames, formatting)
- Simple questions with obvious answers
- Boilerplate generation from clear specs
- File operations and git commands
- Straightforward find-and-replace refactors

**Cost:** ~60x cheaper than Opus

### Sonnet (Balanced)
Use for moderate complexity and multi-step work:
- Multi-file changes following established patterns
- Debugging with clear error messages
- Code review and identifying issues
- Feature implementation from detailed specs
- Documentation and research tasks
- API integrations following documented patterns

**Cost:** ~5x cheaper than Opus

### Opus (Maximum Power)
Use for complex reasoning and architectural decisions:
- Architectural design and system planning
- Complex debugging with no clear error signals
- Large-scale refactoring with design decisions
- Novel problem-solving in uncharted territory
- Security audits and vulnerability analysis
- Performance optimization requiring systemic understanding
- Resolving subtle bugs (race conditions, state management)

**Cost:** Baseline

## Installation

compute-judge is a Claude Code skill distributed as a packaged `.skill` file.

1. Download `compute-judge.skill` from this repository
2. In Claude Code, install it via the skill manager or by placing it in your skills directory
3. Restart Claude Code

Once installed, the `/compute-judge` command will be available in any conversation.

## Usage Example

```
User: /compute-judge fix the typo in README.md

[compute-judge] → haiku | Single-file, mechanical text fix
→ Task dispatched to Haiku
→ Typo fixed and file updated
```

## Classification Reference

For detailed classification criteria and decision shortcuts, see [`references/model-routing.md`](references/model-routing.md).

**Key decision shortcuts:**
- "fix typo" / "rename" / "format" → **Haiku**
- Clear error message + stack trace → **Sonnet**
- "why is this slow?" (no clear cause) → **Opus**
- Single file, mechanical change → **Haiku**
- Multi-file, pattern-based → **Sonnet**
- Multi-file, requires design → **Opus**

## Files

- **SKILL.md** — Detailed skill definition and workflow
- **references/model-routing.md** — Complete model routing heuristics and decision matrix
- **compute-judge.skill** — Packaged distributable skill file

## Philosophy

When in doubt between two tiers, prefer the lower-cost model. It's better to slightly under-provision than to waste tokens on trivial work. Only escalate when the task genuinely demands deeper reasoning.

---

Questions? Check [SKILL.md](SKILL.md) for implementation details or [references/model-routing.md](references/model-routing.md) for routing criteria.
