---
name: engineering-workflow
description: >
  Evidence-based workflow for investigating, planning, implementing,
  and verifying non-trivial software changes.
---

# Engineering workflow

Use this skill for non-trivial engineering tasks.

## Objective

Produce the smallest verified change that satisfies the requested behavior.

## Workflow

1. Understand the task.
2. Inspect the minimum relevant repository context.
3. Identify existing patterns and tests.
4. Produce an evidence-backed plan.
5. Critique the plan only when meaningful risk exists.
6. Implement the approved plan.
7. Run focused verification.
8. Review the final diff.

## Research rules

- Prefer dispatching the researcher and reviewer roles over performing
  repository inspection directly. Keep direct inspection narrow — confirming
  one specific fact — not full evidence gathering or plan critique.
- Search before opening entire files.
- Do not inspect the whole repository.
- Find relevant entry points, symbols, tests, and analogous implementations.
- Separate confirmed repository facts from assumptions.
- Do not edit files during research.
- Persist facts, assumptions, acceptance criteria, and verification commands in
  the task-spec when one is used.

## Plan format

Every plan must contain:

### Goal

One observable result.

### Evidence

Relevant files, symbols, tests, and existing patterns.

### Changes

For each file, describe the intended modification.

### Steps

At most seven implementation steps.

### Risks

Only material risks connected to the task.

### Verification

Exact commands or checks proving completion.

### Out of scope

Changes that must not be made.

## Critique rules

Do not ask whether a reviewer agrees.

The reviewer must look for:

- factual errors;
- missing dependencies;
- edge cases;
- compatibility risks;
- security or data risks;
- unnecessary scope expansion;
- inadequate verification.

Each finding must contain evidence and a minimal correction.
Return APPROVED when no BLOCKER or MAJOR finding exists.

## Build rules

- Confirm that planned files and symbols exist before editing.
- Follow existing project patterns.
- Do not expand scope.
- Do not perform unrelated cleanup.
- Run narrow checks after coherent changes.
- Stop when implementation unexpectedly requires changing architecture,
  public contracts, schemas, security behavior, or acceptance criteria.
- Treat a human approval requirement as a stop condition until the user answers.

## Verification rules

Check in this order:

1. inspect the diff;
2. run affected tests;
3. run type checking;
4. run relevant linting;
5. run broader tests only when necessary.

Do not claim success without executable evidence.

## Retry policy

- Attempt one focused repair for ordinary failures.
- Attempt no more than two repairs for complex tasks.
- Do not repeat planning unless repository evidence invalidates the plan.

## Subagent failure policy

- If a subagent Task call errors, times out, or returns no structured
  output, retry that exact call once with the same task-spec path.
- Never substitute your own work for a failed subagent call and present it
  as that role's independent result. Disclose the failure and any fallback
  in the task-spec, and ask the user how to proceed when the retry also
  fails.

## Review ceremony tiers

- A `standard (light)` task — configuration, dependency manifests,
  documentation, or a read-only assessment, with no source logic, public
  contract, schema, or security changes — may have its plan reviewed by the
  orchestrator directly instead of the reviewer subagent. Record who
  performed the review. Diff review is never skipped or self-performed by
  default; it always goes through the reviewer subagent.

## State handoff

- Use one task-spec per task: `.opencode/tasks/<task-id>.md`.
- Pass the task-spec path to subagents explicitly; do not depend on chat history.
- Keep operational work in the primary agent's todo list and durable task facts in
  the task-spec.
- A prompt-defined workflow is not an enforced gate. Use permissions, the
  question tool, or external automation when enforcement is required.
