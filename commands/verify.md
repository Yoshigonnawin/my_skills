---
description: Run verification commands from a task specification; usage: /verify <task-id>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id>` and read `.opencode/tasks/<task-id>.md`.
Require status `verification`. Invoke verifier through Task with the explicit
task-spec path. Record its factual command results in Build result. Set status to
`done` only when every acceptance criterion is satisfied; otherwise set status to
`blocked` and report the unmet criteria.
