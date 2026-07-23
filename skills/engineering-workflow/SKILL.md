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

- Dispatch the researcher role for repository inspection rather than doing it
  yourself. Keep direct inspection narrow — confirming one specific fact —
  not full evidence gathering. Reviewer dispatch rules are separate (see Ad
  hoc research and review below).
- Search before opening entire files.
- Do not inspect the whole repository.
- Find relevant entry points, symbols, tests, and analogous implementations.
- Separate confirmed repository facts from assumptions.
- Do not edit files during research.
- Persist facts, assumptions, acceptance criteria, and verification commands in
  the task-spec when one is used.
- Close resolvable Assumptions with a narrow follow-up researcher dispatch
  before requesting plan review; reserve reviewer's budget for judgment calls
  a repository or environment check cannot settle.

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

## Ad hoc research and review (plan/build)

Outside the task-spec command chain — a `plan` or `build` primary agent
working directly, without `/prepare` — the following still applies:

- Delegating repository research to `researcher` is mandatory, not optional.
  Dispatch it via `Task` for any non-trivial lookup, repeatedly if needed —
  many narrow researcher calls are cheap. Do not read or grep broad parts of
  the repository yourself; that is the expensive path and belongs to
  researcher.
- Calling `reviewer` is a judgment call, not a gate: when a plan or diff is
  complex or risky enough that an independent check would plausibly catch
  something real, dispatch `reviewer` via `Task` in its ad hoc mode (plan or
  diff described directly in the request, no task-spec file). Do not call it
  reflexively for trivial changes, and do not skip it to save a call when the
  change genuinely warrants a second look.
- This is advisory, not an enforced gate — unlike the task-spec chain, where
  `/implement` technically refuses to proceed without a recorded APPROVED
  verdict. A task with real risk (public contract, schema, security,
  migration, irreversible operation) still belongs in the full
  `/prepare` → `/review-plan` → `/implement` → `/verify` chain, not this path.

## State handoff

- Use one task-spec per task: `.opencode/tasks/<task-id>.md`.
- Pass the task-spec path to subagents explicitly; do not depend on chat history.
- Keep operational work in the primary agent's todo list and durable task facts in
  the task-spec.
- A prompt-defined workflow is not an enforced gate. Use permissions, the
  question tool, or external automation when enforcement is required.
