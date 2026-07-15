---
description: Review a researched task specification; usage: /review-plan <task-id>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id>` and read `.opencode/tasks/<task-id>.md`.
Require status `researched` or `reviewed`. Invoke reviewer through Task with the
explicit task-spec path and request a plan review. Save only evidence-backed
findings in the Review findings section. Set status to `approved` when the result
is APPROVED; otherwise set it to `reviewed` and report the BLOCKER and MAJOR
items that require a decision or correction.
