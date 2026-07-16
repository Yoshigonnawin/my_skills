---
description: Show durable state for a task specification; usage: /task-status <task-id>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id>` and read `.opencode/tasks/<task-id>.md`.
Return its status, risk, unchecked acceptance criteria, open assumptions, the
Plan Review and Diff Review findings separately (BLOCKER and MAJOR items from
each), verification results, deviations, and remaining risks. Do not change
source files.

If the task spec is missing a `## Status` section or the status value is not one
of the canonical statuses, report `unknown` as a diagnostic state and provide
recovery guidance: tell the user to inspect the task spec, determine the actual
lifecycle position, and manually set the status to the correct canonical value
before invoking any lifecycle command. Do not silently guess a lifecycle state.
