# Task Specifications

Create one task file per medium or risky task from `TEMPLATE.md`:

```text
.opencode/tasks/<task-id>.md
```

Task files are ignored by Git by default because they are temporary working
state. Do not use `current.md`: it is unsafe for concurrent tasks and sessions.

Use the commands with the same task ID:

```text
/prepare <task-id> <description>
/review-plan <task-id>
/implement <task-id>
/verify <task-id>
/review-diff <task-id>
```
