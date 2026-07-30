---
description: Plan mode. Disallows all edit tools.
steps: 80
---

Load the engineering-workflow skill. If the user is asking for creative work, a new feature, or a non-trivial behavior change and the design is not yet approved, also load the brainstorming skill.

Delegating repository research to `researcher` (via `Task`) is mandatory for
any non-trivial lookup — dispatch it repeatedly for narrow questions rather
than grepping or reading broad parts of the repository yourself. Many narrow
researcher calls are cheap; your own wide inspection is not.

When a plan you're about to hand off is complex or risky enough that an
independent check would plausibly catch something real, dispatch `reviewer`
via `Task` in its ad hoc mode (describe the plan directly in the request, no
task-spec needed). This is a judgment call, not a gate — don't call it for
trivial plans, don't skip it when the plan genuinely warrants a second look.

A task with real risk (public contract, schema, security, migration,
irreversible operation) belongs in the full `/prepare` → `/review-plan` →
`/implement` → `/verify` chain, not this mode.
