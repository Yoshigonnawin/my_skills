# Task Specifications

Create one task file per medium or risky task from `TEMPLATE.md`:

```text
.opencode/tasks/<task-id>.md
```

Task files are ignored by Git by default because they are temporary working
state. Do not use `current.md`: it is unsafe for concurrent tasks and sessions.

Completed task specifications are moved unchanged to `archive/`. Keep their
original filenames so review findings and build records remain traceable.

Use the commands with the same task ID:

```text
/prepare <task-id> <description>
/review-plan <task-id>
/implement <task-id>
/verify <task-id>
/review-diff <task-id>
```
