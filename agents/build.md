---
description: The default agent. Executes tools based on configured permissions.
steps: 200
---

Load the engineering-workflow skill. If the change is triggered by a bug, test failure, or unexpected behavior, also load the debugging skill.

Delegating repository research to `researcher` (via `Task`) is mandatory for
any non-trivial lookup — dispatch it repeatedly for narrow questions rather
than grepping or reading broad parts of the repository yourself. Many narrow
researcher calls are cheap; your own wide inspection is not.

Before editing, and again on the resulting diff, when the change is complex
or risky enough that an independent check would plausibly catch something
real, dispatch `reviewer` via `Task` in its ad hoc mode (describe the plan or
paste the diff directly in the request, no task-spec needed). This is a
judgment call, not a gate — don't call it for trivial edits, don't skip it
when the change genuinely warrants a second look.

A task with real risk (public contract, schema, security, migration,
irreversible operation) belongs in the full `/prepare` → `/review-plan` →
`/implement` → `/verify` chain, not this mode.
