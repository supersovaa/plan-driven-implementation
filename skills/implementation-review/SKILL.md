---
name: implementation-review
description: Review an implementation attempt in a plan/result workflow according to its durable plan state, focusing on boundary fidelity, recorded outcomes, canonical consistency, and provisional decisions needing user judgment.
---

# Implementation Review

Use this skill as an addition to normal code review for an implementation attempt governed by a durable plan state and plan/result records.
Keep general code-review methodology with the normal review workflow.

## Review according to plan state

Read the current durable plan state before judging the implementation outcome.

For a `completed` plan, verify that the implementation satisfies the plan's purpose, scope, out-of-scope boundaries, constraints, and completion criteria.
Treat missing behavior or evidence required by the current plan contract as acceptance-blocking.

For a `replan-required` plan, review the attempt as an intentional stopped state.
Confirm that implementation stopped at the invalidated boundary, `problem.md` records the causal context and boundary impact needed for replanning, and any `result.md` matches what the attempt actually established.
Judge the stopped state against those responsibilities rather than the invalidated completion criteria.

For a `planned` plan, review the current implementation against the active plan boundary and report what remains before it can be completed.

## Preserve the current plan boundary

Judge acceptance at the current plan boundary without treating literal adherence to an implementation recipe as the goal.
For staged work, keep behavior explicitly assigned to later work with that later stage, including cases where the current stage prepares data, hooks, or temporary behavior for it.
Use subsequent plans or explicit deferred-work records as needed to confirm the boundary and evaluate validation against the behavior owned by the current stage.

When the current change transfers responsibility to deferred follow-up work that is settled enough to plan, verify that the source and destination plans are both updated in the same coherent change, the relevant index and dependency or concurrency metadata are synchronized, and ownership remains unambiguous.
In a pull-request workflow, treat a missing destination plan in the deferral pull request as an acceptance issue unless planning that destination first requires a new upstream requirement or design decision.

When the current plan adds another path to a responsibility already present in repository state, compare the new path with existing paths that establish the same responsibility.
Treat semantic differences within the current plan's required behavior, including relevant boundary conditions, as current-boundary review findings.

Keep plan-relative robustness, observability, or future-stage improvements beyond the current contract as follow-up feedback.
Apply normal code-review acceptance criteria independently.

Check that recorded results correspond to what the implementation actually did and state the implementation outcome directly rather than only as deviations from `plan.md`.
Check that relevant canonical documentation remains semantically consistent with the implementation.

## Surface decisions that need human judgment

Review provisional decisions recorded by the implementer and independently notice important implementation or design decisions that may have been omitted from that list.

The project defines what counts as an important decision requiring user approval.
When such a decision exists, present it to the user clearly enough to approve, reject, or redirect it.

Provisional implementation may continue when the plan boundary remains valid.

## Report actionable review outcomes

Identify:

- violations of the plan boundary;
- state-specific completion or stopped-attempt problems;
- result records that do not match the implementation or describe it only as plan deviations;
- canonical documentation that should be synchronized;
- provisional or newly discovered important decisions requiring user judgment;
- concrete corrections needed before the implementation can be accepted.

Keep review-process mechanics outside this skill.
Editing the PR, committing fixes, pushing branches, and merging are separate workflow responsibilities.
