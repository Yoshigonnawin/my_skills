---
description: Implement an approved task specification; usage: /implement <task-id>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id>` and re-read the task specification at
`.opencode/tasks/<task-id>.md`. Require status `approved`. If risk is
`standard` or `risky`, require that Review Findings contains a Plan Review
entry with verdict `APPROVED`; if it is missing or not `APPROVED`, stop and
report that `/review-plan` must be run first, and do not proceed. This check
does not apply when risk is `simple` and no task-spec review was required.

Confirm planned files and symbols still exist, inspect git status, and set
status to `implementing` before editing any files.

Do not implement the change yourself. Invoke `coder-easy`, `coder-medium`, or
`coder-hard` through Task, chosen by implementation difficulty judged from
Planned Changes (single file with a clear analog -> easy; a few files needing
some judgment -> medium; multiple interacting components or no existing
analog -> hard), not from task risk. Always use `coder-hard` for `risky` work
after the human gate regardless of apparent diff size. Pass the explicit
task-spec path and require the subagent to implement only the planned scope.
If the subagent reports that the assigned tier was too easy for the actual
work, re-dispatch to the next tier instead of accepting a partial result.

If implementation was interrupted, re-read the task specification before
resuming to confirm the persisted status; refuse to resume unless status is
`implementing`, and resume only from `implementing`, asking the user to resolve
any other state.

If the subagent reports that implementation requires changing public APIs,
schemas, architecture, security behavior, or acceptance criteria, stop and
ask the user instead of proceeding. Update Changed files, Deviations, and
Remaining risks in the task-spec from the subagent's report, then set status
to `verification` only after completion of all planned work.
