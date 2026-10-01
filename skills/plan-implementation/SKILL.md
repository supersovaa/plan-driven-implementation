---
name: plan-implementation
description: Implement one explicitly selected repository plan in one implementation PR while preserving its boundary, auditing completion before marking it complete, recording a self-contained implementation result, and routing an invalidated plan through replanning.
---

# Plan Implementation

Use this skill for one implementation PR that implements one explicitly selected implementation plan.
The plan is a repository-persistent implementation contract, not an informal task description.

## Read the planning material before implementation

Before changing implementation code, read the selected `plan.md`, the `index.md` that owns its state and dependencies, referenced requirement and design canon, and relevant repository-level implementation rules.
Treat the explicitly relevant base branch as the source of confirmed plan state.
Use the current plan and current canon for normal implementation; linked planning history is replanning context.
Implement the selected plan only when its durable state is `planned` and its dependencies are satisfied in confirmed repository state.
Route a `replan-required` plan through implementation planning before implementation; a completed plan remains a completion record.

Use one implementation task and one implementation PR for the selected plan.
Handle another plan as a separate implementation task and implementation PR.

## Preserve the plan boundary, not an implementation recipe

Adapt implementation details to the current repository as needed.
A stale implementation assumption does not require replanning if the plan's purpose, scope, out-of-scope work, and completion criteria can still be preserved.
Once implementation of a selected plan begins, treat that plan as fixed for that attempt.
Resolve implementation-time discoveries by adapting the implementation within the existing plan boundary and recording the final outcome in `result.md`.
Keep the active `plan.md` unchanged for that implementation attempt.

Within that boundary, make implementation and design decisions autonomously.
When a project defines which kinds of canon may be changed, follow that policy.
If no project policy exists, use a conservative default: design canon may be updated within the plan boundary, while requirements, rules, and external-specification canon are not changed by default.

If completing the work requires changing the plan's purpose, scope, out-of-scope work, or completion criteria, stop that plan as `replan-required` instead of silently redefining it.
Other plans keep their current state unless their own boundaries are known to require replanning.

## Record what actually happened

Create or update `result.md` when an execution result exists.
Write it as the final execution record for later readers.
State the resulting implementation directly so the completed or stopped outcome can be understood from `result.md` alone.
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
Permanent decisions that matter beyond this work unit belong in the appropriate canon; record in the result that the decision was made and what canon was updated.
Treat `plan.md` as planning history and implementation-audit input rather than required reading for understanding the recorded result.

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

## Complete the plan

Once its completion audit passes:

- validate that plan;
- finalize its `result.md`;
- update relevant canon and indexes so the repository is semantically consistent;
- set its durable state to `completed`;
- create a commit that contains the plan's coherent completed state.

A plan may use multiple commits, but its completion commit must include the result, completed state, and required documentation synchronization.

Parent plans are completed by their own integration-level completion criteria and their own result; child completion alone does not automatically complete the parent.

## Stop using an invalidated plan

If a plan becomes `replan-required`, stop implementing against that plan.
Record the causal replanning context in `problem.md`, set the affected plan to `replan-required`, and finish the implementation PR as a coherent stopped attempt.

Before that implementation PR is merged, keep unfinished work for the invalidated plan outside confirmed repository state.
The surrounding repository workflow may preserve that work separately for possible reuse; replanning does not require discarding it.

The replacement planning PR starts only after the repository workflow has merged the stopped implementation PR.
Treat a user instruction to replan as sufficient to create that planning PR once the merge has occurred; merging the implementation PR remains a repository-workflow decision.
Base the planning PR on the updated confirmed base state.

Create or update `result.md` when the attempt produced an execution result.
Do not create a result merely to record that replanning was required.

When the same responsibility is replanned, the planning workflow preserves the superseded planning context as linked history and makes the replacement plan current.
Before reusing preserved unfinished work, compare it with the replacement plan and the confirmed repository state that plan is based on; reuse only the parts that remain consistent with both.

## Keep documentation navigable

When this skill adds, deletes, moves, or renames planning/result documents, update the relevant `index.md` in the same change.
Do not leave a newly created `result.md` or `problem.md` unreachable from the repository's documentation structure when that structure uses indexes.

This skill defines implementation semantics within an implementation PR.
Commit delivery, push, merge, and other repository-hosting operations belong to the appropriate repository workflow.
