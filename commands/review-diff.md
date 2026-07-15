---
description: Review the current diff against a task specification; usage: /review-diff <task-id>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id>` and read `.opencode/tasks/<task-id>.md`.
Invoke reviewer through Task with the explicit task-spec path and request a diff
review against its acceptance criteria. Record findings in Review findings. Do
not claim the task is complete solely because review returns APPROVED.
