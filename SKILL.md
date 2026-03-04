---
name: compute-judge
description: Intelligent model router that selects the optimal Claude model (haiku, sonnet, opus) for a task and dispatches it automatically. Use when the user runs /compute-judge followed by a task description. Optimizes token spend by routing simple tasks to haiku, moderate tasks to sonnet, and complex tasks to opus. Triggers on explicit /compute-judge invocation only.
---

# Compute Judge

Route tasks to the optimal model and execute them automatically.

## Workflow

1. Read the user's task description (passed as arguments after `/compute-judge`)
2. Read `references/model-routing.md` for classification criteria
3. Classify the task into one of three tiers: **haiku**, **sonnet**, or **opus**
4. Report the classification briefly: `[compute-judge] → <model> | <one-line reason>`
5. Dispatch the task using the **Agent tool** with the selected `model` parameter

## Classification Process

Evaluate the task against these dimensions (in order of importance):

1. **Ambiguity** — Is the task clearly specified or open-ended?
2. **Scope** — How many files/systems are involved?
3. **Reasoning depth** — Does it need pattern-matching or novel judgment?
4. **Risk** — Could a wrong answer cause harm (security, data loss)?

Apply the lowest-cost model that can handle the task competently. When uncertain between two tiers, prefer the cheaper model — it is better to slightly under-provision than to waste tokens on trivial work.

## Dispatching

Use the Agent tool with:
- `model`: the selected model (`"haiku"`, `"sonnet"`, or `"opus"`)
- `subagent_type`: `"general-purpose"`
- `prompt`: the user's full task description, including any relevant context from the conversation

The subagent receives the task and executes it end-to-end. Do not re-do the work yourself after dispatching.

## Edge Cases

- If no task description is provided, ask the user what they want to do
- If the task is ambiguous enough that classification is unclear, default to **sonnet**
- If the user disagrees with the routing, respect their override
