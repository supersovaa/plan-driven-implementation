---
name: plan-implementation
description: Implement explicitly selected repository plans while preserving plan boundaries, proving completion criteria before marking plans complete, recording implementation decisions and results, and stopping cleanly when replanning is required.
---

# Plan Implementation

Use this skill to implement one or more explicitly selected implementation plans.
The plans are repository-persistent implementation contracts, not informal task descriptions.

## Read the planning material before implementation

Before changing implementation code, read the selected `plan.md`, the `index.md` that owns its state and dependencies, referenced requirement and design canon, and relevant repository-level implementation rules.
Treat the explicitly relevant base branch as the source of confirmed plan state.

Implement only plans explicitly selected for the current task.
Do not add another plan merely because it is currently implementable.
If a selected plan depends on unfinished work that is not selected for the current task, do not implement that plan; independent selected plans may still proceed.

Multiple selected plans may be implemented in one session or one PR.
Respect their dependency order and keep each plan as an independent completion boundary.
A plan completed earlier in the same task may satisfy a dependency of a later selected plan even though it has not yet been merged to the base branch.

## Preserve the plan boundary, not an implementation recipe

Adapt implementation details to the current repository as needed.
A stale implementation assumption does not require replanning if the plan's purpose, scope, out-of-scope work, and completion criteria can still be preserved.
Once implementation of a selected plan begins, treat that plan as fixed for that attempt.
Do not rewrite it to accommodate implementation decisions.

Within that boundary, make implementation and design decisions autonomously.
When a project defines which kinds of canon may be changed, follow that policy.
If no project policy exists, use a conservative default: design canon may be updated within the plan boundary, while requirements, rules, and external-specification canon are not changed by default.

If completing the work requires changing the plan's purpose, scope, out-of-scope work, or completion criteria, stop that plan as `replan-required` instead of silently redefining it.
Plans that do not depend on the stopped plan may continue.
A dependent plan remains `planned` unless its own boundary is also known to require replanning.

## Record what actually happened

Create or update `result.md` when an execution result exists.
Its structure is repository-defined, but it should capture the implementation facts that matter, including as applicable:

- what was actually implemented;
- non-trivial implementation decisions, with a short reason;
- provisional decisions that may require later user approval;
- meaningful deviations from the plan that remain within its boundary;
- the actual form of permitted temporary implementation;
- validation performed and its outcome;
- incomplete or follow-up work.

Do not restate canonical requirements or design in `result.md`.
Permanent decisions that matter beyond this work unit belong in the appropriate canon; record in the result that the decision was made and what canon was updated.

A provisional decision is a decision made so implementation can proceed even though it may be important enough for later user approval.
Keep provisional decisions distinguishable from ordinary implementation decisions in `result.md`.
Do not model approval status inside `result.md`; record that the decision was provisional at implementation time.

## Audit completion before marking a plan completed

Immediately before setting a selected plan to `completed`, reread its `plan.md` and perform an explicit completion audit.
Do not infer completion merely because major types, APIs, or helper functions exist, or because the existing test suite passes.

Check at least these parts of the plan item by item:

- implementation scope;
- implementation constraints that impose requirements on the implementation result;
- completion criteria.

For every applicable item, identify both concrete implementation evidence showing that the required responsibility is actually implemented and concrete validation evidence showing that the requirement was verified.

For a completion criterion that requires behavior, the existence of a type, API, or helper function is not sufficient evidence of completion.
Distinguish implementing foundational types from establishing the foundational responsibility required by the plan.

When an item requires actual progression or integration, such as connecting, retaining, exposing, resuming, advancing automatically, or applying behavior to existing processing, verify that the required path is actually connected and that the behavior occurs as required by the current plan.

Read subsequent plans to confirm responsibility boundaries.
A later plan's intent to use the current plan's foundation does not move implementation scope or completion criteria explicitly assigned to the current `plan.md` into that later plan.
Conversely, do not add usage or integration that is assigned for the first time to a later plan to the current plan's completion criteria.

A passing test suite is supporting evidence, not a substitute for item-by-item completion evidence.
Mark the plan `completed` only when every applicable audited item has sufficient implementation evidence and validation evidence.

If the completion audit finds an unmet item:

- continue implementation when it can be satisfied within the current plan boundary;
- use `replan-required` under the existing policy when satisfying it requires changing the plan boundary;
- if work must end without requiring replanning, do not mark the plan `completed`; record the incomplete work accurately in `result.md`.

State that there is no incomplete work in `result.md` only when the completion audit confirms implementation and validation evidence for every applicable item.

## Complete one plan at a time

For each selected plan, once its completion audit confirms sufficient implementation and validation evidence for every applicable audited item:

- validate that plan;
- finalize its `result.md`;
- update relevant canon and indexes so the repository is semantically consistent;
- set its durable state to `completed`;
- create a commit that contains the plan's coherent completed state.

A plan may use multiple commits, but its completion commit must include the result, completed state, and required documentation synchronization.
Do not wait until all selected plans are finished before recording earlier completed plans.

Parent plans are completed by their own integration-level completion criteria and their own result; child completion alone does not automatically complete the parent.

## Stop cleanly when replanning is required

If a plan becomes `replan-required`, revert the unfinished changes made for that plan, including implementation, tests, and canon edits attributable to that stopped attempt.
Do not revert already completed independent plans.

Keep `result.md` concise: record that execution stopped and link to `problem.md`.
Put the detailed replanning report in `problem.md`, including what was attempted, what invalidated the current plan boundary, what needs replanning, and that the unfinished changes were reverted.
Set the plan state to `replan-required` and commit the coherent stopped state.

When the same work unit is later replanned and completed, obsolete stopped-attempt documents are removed by the planning workflow; Git history preserves them.

## Keep documentation navigable

When this skill adds, deletes, moves, or renames planning/result documents, update the relevant `index.md` in the same change.
Do not leave a newly created `result.md` or `problem.md` unreachable from the repository's documentation structure when that structure uses indexes.

This skill defines implementation semantics only.
If the task also requires commit delivery, push, or pull-request creation beyond the plan-completion commits described here, hand off to the appropriate repository workflow rather than embedding a specific hosting workflow in this skill.
