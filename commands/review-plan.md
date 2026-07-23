---
description: Review a researched task specification; usage: /review-plan <task-id>
agent: orchestrator
---

Parse `$ARGUMENTS` as `<task-id>` and read `.opencode/tasks/<task-id>.md`.
Require status `researched` or `reviewed`.

First scan Assumptions for anything resolvable by repository or
environment inspection rather than genuine design judgment. Dispatch
researcher again with a narrow, specific question for each such gap, and
move the resolved item into Confirmed Facts. Then invoke reviewer through
Task with the explicit task-spec path and
instruct it to operate in **plan review mode**: read only the task-spec, do
not run `git diff`, there is no implementation yet. If the Task call errors
or returns no structured output, retry once with the same task-spec path
before falling back to the orchestrator's subagent-failure policy (disclose,
never silently self-substitute). Save only evidence-backed findings, plus
`Performed by: reviewer subagent`, in the Plan Review subsection of Review
Findings.

Persist the verdict: set status to `approved` when the verdict is APPROVED;
otherwise set status to `reviewed` and report the BLOCKER and MAJOR items that
require a decision or correction.
