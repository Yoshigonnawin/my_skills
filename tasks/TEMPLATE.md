# Task: <task-id>

## Status

`draft`

Valid statuses: `draft`, `researched`, `reviewed`, `approved`, `implementing`, `verification`, `done`, `blocked`, `unknown`.

- `draft` — task spec created, not yet researched.
- `researched` — researcher has gathered repository evidence; ready for plan review.
- `reviewed` — plan review returned CHANGES REQUIRED; awaiting corrections and re-review.
- `approved` — plan review returned APPROVED; ready for implementation.
- `implementing` — implementation in progress; an interrupted session stays here.
- `verification` — implementation complete; verification commands pending.
- `done` — all acceptance criteria verified; task complete.
- `blocked` — verification failed or a human decision is required.
- `unknown` — diagnostic fallback when status is missing or invalid; lifecycle commands must require explicit user-directed recovery rather than guessing.

## Risk

`simple | standard | risky`

## Goal

One observable behavior change.

## Constraints

- 

## Repository Evidence

- 

## Confirmed Facts

- 

## Assumptions

- 

## Planned Changes

1. 

## Acceptance Criteria

- [ ] 

## Verification

```bash
uv run ruff check .
```

## Out Of Scope

- 

## Review Findings

### Plan Review

- Performed by: 
- Not yet reviewed.

### Diff Review

- Performed by: 
- Not yet reviewed.

## Build Result

- Changed files:
- Commands executed:
- Deviations:
- Remaining risks:
