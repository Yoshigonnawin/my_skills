---
name: brainstorming
description: Use before creative work — creating features, components, or modifying behavior — to explore intent and design before implementation. Load after engineering-workflow.
---

# Brainstorming supplement

Use this skill together with the engineering-workflow skill.

## When to use

- The user asks for a new feature, component, or non-trivial behavior change.
- The task scope or success criteria are unclear.
- There are multiple plausible approaches.

## Process

1. **Understand the request**: purpose, constraints, success criteria, out-of-bounds items.
2. **Explore the existing codebase** for relevant patterns and boundaries.
3. **Ask focused questions** one at a time until the goal is clear.
4. **Propose 2-3 approaches** with trade-offs and a recommendation.
5. **Get user approval** on the chosen approach before writing a plan.
6. **After approval**, hand off to the engineering-workflow skill (use `/prepare` or dispatch `researcher`) for evidence gathering and task-spec creation.

## Stop conditions

- Do not write implementation code or create files until the design is approved.
- If the request is too large for a single task, decompose it with the user first.
- Do not expand scope beyond what serves the current goal.
