---
description: Review the current diff against a task specification; usage: /review-diff <task-id>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id>` and read `.opencode/tasks/<task-id>.md`.
Require status `implementing` or later. Invoke reviewer through Task with the
explicit task-spec path and instruct it to operate in **diff review mode**: read
the task-spec, run `git diff` against the working tree, and compare the diff
against Planned Changes, Out of Scope, and Acceptance Criteria. Record findings
in the Diff Review subsection of Review Findings. Persist the verdict: on
APPROVED, set status to `verification`; on CHANGES REQUIRED, set status to `implementing`. Do not claim the task is complete solely because the verdict is APPROVED.
