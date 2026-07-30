---
name: debugging
description: Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes. Load after engineering-workflow.
---

# Debugging supplement

Use this skill together with the engineering-workflow skill.

## Core rule

Find the root cause before proposing a fix. Symptom-only fixes are not acceptable.

## Process

1. **Reproduce** the failure consistently. Record exact steps, environment, and inputs.
2. **Read errors fully**: stack traces, line numbers, error codes, recent logs.
3. **Check recent changes**: `git diff`, recent commits, dependency or config changes.
4. **Trace data flow** backwards from the failure to its source.
5. **Find a working analog** in the same codebase and compare differences.
6. **Form one hypothesis** and test it with the smallest possible change.
7. **Verify** the fix with the failing test or reproduction.

## Stop conditions

- If you have tried 3+ hypotheses and none worked, stop and ask the user whether the architecture itself should be questioned.
- Do not bundle unrelated refactoring with the fix.
- Do not skip writing or running a reproduction test when one is feasible.

## Output

Return: root cause, evidence (file/line), hypothesis tested, fix applied, and verification result.
