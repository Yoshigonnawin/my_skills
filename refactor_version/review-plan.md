---
description: Review a researched task specification; usage: /review-plan <task-id>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id>` and read `.opencode/tasks/<task-id>.md`.
Require status `researched` or `reviewed`. Invoke reviewer through Task with the
explicit task-spec path and instruct it to operate in **plan review mode**: read
only the task-spec, do not run `git diff`, there is no implementation yet. Save
only evidence-backed findings in the Plan Review subsection of Review Findings.
Set status to `approved` when the verdict is APPROVED; otherwise set it to
`reviewed` and report the BLOCKER and MAJOR items that require a decision or
correction.
