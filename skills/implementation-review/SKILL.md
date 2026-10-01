---
name: implementation-review
description: Review an implementation that follows a plan/result workflow, focusing on plan-boundary fidelity, recorded results, canonical consistency, and provisional decisions needing user judgment.
---

# Implementation Review

Use this skill as an addition to normal code review when the repository uses `plan.md` / `result.md` implementation records.
Do not turn this skill into a general code-review methodology.

## Review the implementation contract

Check whether the implementation satisfies the current plan's purpose, scope, out-of-scope boundaries, constraints, and completion criteria without treating literal adherence to an implementation recipe as the goal.

Judge acceptance at the current plan boundary.
For staged work, keep behavior explicitly assigned to later work with that later stage, including cases where the current stage prepares data, hooks, or temporary behavior for it.
Use subsequent plans or explicit deferred-work records as needed to confirm the boundary and evaluate validation against the behavior owned by the current stage.

Treat a review finding as acceptance-blocking when the current plan contract requires the missing behavior or evidence.
Keep additional robustness, observability, or future-stage behavior as non-blocking feedback or follow-up work.

Check that the recorded result corresponds to what the implementation actually did and that relevant canonical documentation remains semantically consistent with the implementation.

## Surface decisions that need human judgment

Review provisional decisions recorded by the implementer and independently notice important implementation or design decisions that may have been omitted from that list.

The project, not this skill, defines what counts as an important decision requiring user approval.
When such a decision exists, present it to the user clearly enough to approve, reject, or redirect it.

Do not require the implementer to have stopped merely because an important decision was needed; provisional implementation is allowed when the plan boundary remained valid.

## Report actionable review outcomes

Identify:

- violations of the plan boundary;
- result records that do not match the implementation;
- canonical documentation that should be synchronized;
- provisional or newly discovered important decisions requiring user judgment;
- concrete corrections needed before the implementation can be accepted.

Keep review-process mechanics outside this skill.
Editing the PR, committing fixes, pushing branches, and merging are separate workflow responsibilities.
