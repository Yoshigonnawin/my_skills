---
description: Create and research a task specification; usage: /prepare <task-id> <description>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id> <description>`. Validate that task-id is a
lowercase kebab-case identifier. If the description is absent, ask for it. If
`.opencode/tasks/<task-id>.md` already exists, stop and ask whether to continue
that task instead of overwriting it.

Create the file from `.opencode/tasks/TEMPLATE.md`, set status to `draft`, and
record the description. Invoke researcher through Task with the task description
and the explicit task-spec path. Incorporate repository evidence into the
task-spec, set status to `researched`, and report the path and open assumptions.
