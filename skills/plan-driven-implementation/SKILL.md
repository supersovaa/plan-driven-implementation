---
name: plan-driven-implementation
description: Execute one explicitly selected implementation plan while preserving its boundary, recording the actual result, auditing completion, and stopping cleanly when the boundary requires replanning.
---

# Plan-Driven Implementation

Use this skill to execute one explicitly selected implementation plan.
Begin from the selected plan and established execution readiness.

## Read the implementation contract

Before changing implementation code, read the selected `plan.md`, the `index.md` that owns its state, referenced requirement and design canon, and relevant repository-level implementation rules.
Treat the current plan and current canon as authoritative for this attempt.
Keep the selected plan as the sole implementation boundary for the attempt.

## Preserve the plan boundary

Adapt implementation details to the current repository as needed.
A stale implementation assumption may be adapted when it is not a settled plan constraint and the plan's purpose, scope, out-of-scope work, settled constraints, and completion criteria remain valid.
This may include restructuring implementation established by a dependency or ancestor plan when the selected plan's contract and current canon remain valid.
Treat the selected plan as fixed for the attempt.
Resolve implementation-time discoveries within the existing boundary and record the resulting implementation in `result.md`.

Within the plan boundary, make implementation and design decisions autonomously.
When a project defines which kinds of canon may be changed, follow that policy.
If no project policy exists, use a conservative default: design canon may be updated within the plan boundary, while requirements, rules, and external-specification canon stay with their owning workflow.

When completing the work requires changing the plan's purpose, scope, out-of-scope work, settled constraints, or completion criteria, move the plan to `replan-required`.

## Record what actually happened

Create or update `result.md` when an execution result exists.
Record the resulting implementation directly rather than expressing it only as deviations from `plan.md`.
Its structure is repository-defined, but it should capture the implementation facts that matter, including as applicable:

- what was actually implemented;
- whether the attempt completed the planned work, or what remains;
- non-trivial implementation decisions, with a short reason;
- provisional decisions that may require later user approval;
- implementation-time discoveries that materially shaped the final implementation;
- the actual form of permitted temporary implementation;
- validation performed and its outcome;
- follow-up work.

Reference canonical requirements or design instead of restating them in `result.md`.
Permanent decisions that matter beyond this work unit belong in the appropriate canon; record in the result what canon was updated.

A provisional decision is a decision made so implementation can proceed even though it may be important enough for later user approval.
Keep provisional decisions distinguishable from ordinary implementation decisions in `result.md`.
Keep approval status with its owning workflow rather than modeling it in `result.md`.

## Audit completion

Immediately before completing the selected plan, reread its `plan.md` and audit these items individually:

- implementation scope;
- implementation constraints that impose requirements on the implementation result;
- completion criteria.

For every applicable item, confirm concrete implementation evidence and validation evidence.

For behavioral or integration responsibilities, confirm the required path actually works.
Types, APIs, helpers, and passing test suites may support that evidence; behavioral and integration responsibilities require evidence of the resulting behavior.

Use subsequent plans only to confirm responsibility boundaries.
Keep responsibilities assigned to the current plan in the current plan, and behavior assigned to later plans with those later plans.

When every audited item has sufficient evidence, set the plan to `completed`.
When an audited item remains unmet, continue within the current boundary, leave the attempt incomplete, or use `replan-required` when the boundary must change.

## Finalize a completed plan

After the completion audit passes:

- validate the plan;
- finalize its `result.md`;
- update relevant canon and indexes so the repository is semantically consistent;
- set its durable state to `completed` from the implementation contract and completion evidence;
- leave a coherent completed state containing the result, completed state, and required documentation synchronization.

The surrounding workflow derives incorporation eligibility from the recorded merge-prerequisite conditions.

Parent plans complete against their own integration-level completion criteria.

## Stop at an invalidated boundary

When the plan boundary becomes invalid, stop implementation against that plan.
Record the causal context in `problem.md`, set the plan to `replan-required`, and finish the attempt as a coherent stopped state.

Create or update `result.md` when the attempt produced an execution result.
A replanning requirement by itself does not create an execution result.
Keep `problem.md` focused on the assumptions that failed and the boundary impact needed by later replanning.

## Keep documentation navigable

When this skill adds, deletes, moves, or renames planning or result documents, update the relevant `index.md` in the same change.
Keep newly created `result.md` and `problem.md` reachable from the repository's documentation structure when that structure uses indexes.

This skill owns implementation semantics for the selected plan.
Planning and review belong to their respective skills.
