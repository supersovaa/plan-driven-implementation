---
name: plan-implementation
description: Implement explicitly selected repository plans while preserving plan boundaries, auditing completion before marking plans complete, recording implementation decisions and results, and routing invalidated plans through replanning.
---

# Plan Implementation

Use this skill to implement one or more explicitly selected implementation plans.
The plans are repository-persistent implementation contracts, not informal task descriptions.

## Read the planning material before implementation

Before changing implementation code, read the selected `plan.md`, the `index.md` that owns its state and dependencies, referenced requirement and design canon, and relevant repository-level implementation rules.
Treat the explicitly relevant base branch as the source of confirmed plan state.
Use the current plan and current canon for normal implementation; linked planning history is replanning context.
Implement only selected plans whose durable state is `planned`.
Route `replan-required` plans through implementation planning before implementation; completed plans remain completion records.

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

Immediately before completing a selected plan, reread its `plan.md` and audit these items individually:

- implementation scope;
- implementation constraints that impose requirements on the implementation result;
- completion criteria.

For every applicable item, confirm concrete implementation evidence and validation evidence.

For behavioral or integration responsibilities, including connecting, retaining, exposing, resuming, advancing automatically, or applying behavior to existing processing, confirm that the required path actually works.
Types, APIs, helpers, and passing test suites may support that evidence; behavioral and integration responsibilities require evidence of the resulting behavior.

Use subsequent plans to confirm responsibility boundaries.
Responsibilities assigned to the current plan remain in the current plan, while usage or integration first assigned to a later plan remains in that later plan.

Mark the plan `completed` only when every audited item has sufficient implementation and validation evidence.

When the audit finds an unmet item, continue implementation within the current boundary, use `replan-required` when the boundary must change, or end the work with the plan incomplete and record the remaining work in `result.md`.

`result.md` may state that no incomplete work remains after the audit passes.

## Complete one plan at a time

For each selected plan, once its completion audit passes:

- validate that plan;
- finalize its `result.md`;
- update relevant canon and indexes so the repository is semantically consistent;
- set its durable state to `completed`;
- create a commit that contains the plan's coherent completed state.

A plan may use multiple commits, but its completion commit must include the result, completed state, and required documentation synchronization.
Do not wait until all selected plans are finished before recording earlier completed plans.

Parent plans are completed by their own integration-level completion criteria and their own result; child completion alone does not automatically complete the parent.

## Stop using an invalidated plan

If a plan becomes `replan-required`, stop implementing against that plan and update its durable state so it is no longer treated as implementable.

Record the replanning context in `problem.md`, including what invalidated the current plan boundary and the resulting impact on that boundary.
When an implementation attempt already produced facts worth retaining, include the attempted work needed to understand that impact.
Keep this record causal and descriptive so later planning can derive a fresh boundary from current canon.

Do not require unfinished implementation changes to be reverted solely because replanning is required.
A surrounding repository or delivery workflow may retain, isolate, or discard those changes.
Retained unfinished changes are working material rather than a result of the invalidated plan, and they must not be treated as confirmed implementation state.
Keep retained unfinished changes outside the validation, result, and completion boundary of other plans until a replacement plan accepts them.

Create or update `result.md` only when the attempt produced an execution result worth recording.
Do not create a result merely to record that replanning was required.

When the same responsibility is later replanned, the planning workflow preserves the superseded planning context as linked history and makes the replacement plan current.
Before reusing retained unfinished changes, compare them with the replacement plan and the confirmed repository state that plan is based on; reuse only the parts that remain consistent with both.

## Keep documentation navigable

When this skill adds, deletes, moves, or renames planning/result documents, update the relevant `index.md` in the same change.
Do not leave a newly created `result.md` or `problem.md` unreachable from the repository's documentation structure when that structure uses indexes.

This skill defines implementation semantics only.
If the task also requires commit delivery, push, or pull-request creation beyond the plan-completion commits described here, hand off to the appropriate repository workflow rather than embedding a specific hosting workflow in this skill.
