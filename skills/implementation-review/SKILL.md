---
name: implementation-review
description: Review an implementation that follows a plan/result workflow, focusing on plan-boundary fidelity, recorded results, canonical consistency, and provisional decisions needing user judgment.
---

# Implementation Review

Use this skill as an addition to normal code review when the repository uses `plan.md` / `result.md` implementation records.
Keep general code-review methodology with the normal review workflow.

## Review according to plan state

Read the current durable plan state before judging the implementation outcome.

For a `completed` plan, verify that the implementation satisfies the plan's purpose, scope, out-of-scope boundaries, constraints, and completion criteria.
Treat missing behavior or evidence required by the current plan contract as acceptance-blocking.

For a `replan-required` plan, review the attempt as an intentional stopped state.
Confirm that implementation stopped at the invalidated boundary, `problem.md` records the causal context and boundary impact needed for replanning, and any `result.md` matches what the attempt actually established.
Judge the stopped state against those responsibilities rather than the superseded completion criteria.

For a `planned` plan, review the current implementation against the active plan boundary and report what remains before it can be completed.

## Preserve the current plan boundary

Judge acceptance at the current plan boundary without treating literal adherence to an implementation recipe as the goal.
For staged work, keep behavior explicitly assigned to later work with that later stage, including cases where the current stage prepares data, hooks, or temporary behavior for it.
Use subsequent plans or explicit deferred-work records as needed to confirm the boundary and evaluate validation against the behavior owned by the current stage.

Keep plan-relative robustness, observability, or future-stage improvements beyond the current contract as follow-up feedback.
Apply normal code-review acceptance criteria independently.

Check that recorded results correspond to what the implementation actually did and that relevant canonical documentation remains semantically consistent with the implementation.

## Surface decisions that need human judgment

Review provisional decisions recorded by the implementer and independently notice important implementation or design decisions that may have been omitted from that list.

The project defines what counts as an important decision requiring user approval.
When such a decision exists, present it to the user clearly enough to approve, reject, or redirect it.

Provisional implementation may continue when the plan boundary remains valid.

## Report actionable review outcomes

Identify:

- violations of the plan boundary;
- state-specific completion or stopped-attempt problems;
- result records that do not match the implementation;
- canonical documentation that should be synchronized;
- provisional or newly discovered important decisions requiring user judgment;
- concrete corrections needed before the implementation can be accepted.

Keep review-process mechanics outside this skill.
Editing the PR, committing fixes, pushing branches, and merging are separate workflow responsibilities.
