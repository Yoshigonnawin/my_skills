---
description: Implement an approved task specification; usage: /implement <task-id>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id>` and re-read the task specification at
`.opencode/tasks/<task-id>.md`. Require status `approved`. Confirm planned files
and symbols still exist, inspect git status, and set status to `implementing` before editing any files. Implement only the planned scope.

If implementation was interrupted, re-read the task specification before
resuming to confirm the persisted status; refuse to resume unless status is
`implementing`, and resume only from `implementing`, asking the user to resolve
any other state.

If implementation requires changing public APIs, schemas, architecture, security
behavior, or acceptance criteria, stop and ask the user. Update Changed files,
Deviations, and Remaining risks in the task-spec, then set status to
`verification` only after completion of all planned work.
