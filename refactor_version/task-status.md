---
description: Show durable state for a task specification; usage: /task-status <task-id>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id>` and read `.opencode/tasks/<task-id>.md`.
Return its status, risk, unchecked acceptance criteria, open assumptions, the
Plan Review and Diff Review findings separately (BLOCKER and MAJOR items from
each), verification results, deviations, and remaining risks. Do not change
source files.
