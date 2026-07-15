---
description: Implement an approved task specification; usage: /implement <task-id>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id>` and read `.opencode/tasks/<task-id>.md`.
Require status `approved`. Confirm planned files and symbols still exist, inspect
git status, and set status to `implementing`. Implement only the planned scope.

If implementation requires changing public APIs, schemas, architecture, security
behavior, or acceptance criteria, stop and ask the user. Update Changed files,
Deviations, and Remaining risks in the task-spec, then set status to
`verification`.
