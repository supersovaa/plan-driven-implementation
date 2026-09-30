---
name: implementation-planning
description: Turn already-settled requirements and design into small, repository-persistent implementation plans with explicit boundaries, dependencies, and durable state.
---

# Implementation Planning

Use this skill after the relevant requirements and design decisions are already settled.
Do not use it to perform product or architecture design that should be decided separately.

## Establish the repository state first

Before choosing the next work, inspect the latest state of the explicitly relevant base branch.
Treat that branch as the source of confirmed plan state.

Use repository conventions when they already define where implementation plans live and how they are indexed.
Otherwise, prefer semantic work directories under `docs/implementation/`.
Do not use dates or sequence numbers as the primary identity of a work unit.

## Plan small implementation boundaries

Prefer work units that:

- have one clear implementation purpose;
- can be completed and validated independently;
- are small enough that replanning does not require preserving a large partial implementation;
- avoid pulling future work into the current scope merely because it may be useful later.

Do not split work mechanically just to reduce file count or diff size.
Create a `plan.md` only for a node that has an independent implementation boundary.
Pure organizational directories may have only an `index.md`.

A parent may have its own `plan.md` only when it has an independent integration-level objective and completion criteria beyond being a container for child plans.

## Keep plans about the implementation contract

A plan should define, in whatever structure fits the repository:

- the purpose of the work;
- references to the relevant requirement and design canon;
- implementation scope;
- out-of-scope work;
- the required behavior of temporary implementation, when a stage will be completed later;
- constraints that are already settled and materially affect implementation;
- what the implementer may decide autonomously;
- completion criteria.

Do not copy canonical requirements or design into the plan merely to make it self-contained.
Do not prescribe files, types, functions, algorithms, or implementation order unless those details are already settled constraints.
The plan defines what must be established and the boundary of the work; implementation mechanics normally belong to the implementer.

## Plan deferred stages explicitly

When a use case should flow end to end before one stage reaches its final implementation, decide that staged approach in the plan.
For each deferred stage, state:

- what behavior the temporary implementation must provide so the surrounding flow can be implemented and validated;
- what full behavior is deferred to later work.

Let the implementer choose the simplest temporary form that satisfies that contract.
When a particular temporary form is already a settled constraint, record it with the other implementation constraints.
Place the deferred full behavior in separate follow-up work when it is already settled enough to plan.

## Record dependencies centrally

Determine plan dependencies and record them in the nearest common `index.md` for the plans they relate to.
The index should make it possible to identify each current plan, its durable state, and its direct dependencies, so implementable work can be derived from facts rather than stored as a separate `ready` flag.

Use only these durable plan states unless an existing repository convention provides an equivalent model:

- `planned`
- `completed`
- `replan-required`

Do not persist transient execution state such as `in-progress` or derived state such as `blocked`.

## Replanning

When a user directs replanning while an implementation PR is in scope, first use the repository workflow to finish and merge that implementation PR as a coherent stopped attempt. The merged state records the affected plan as `replan-required` and carries the causal `problem.md` context while leaving unfinished implementation work outside confirmed repository state. After that merge, create the replacement planning PR from the updated base branch. Treat the replanning instruction as sufficient to invoke this sequence without requiring a separate request to open the planning PR.

When a user directs a minor plan correction that preserves the current plan boundary, create and merge the planning correction before the implementation PR continues. The implementation PR then updates to the new base and follows the corrected current plan without entering `replan-required`.

When a plan is `replan-required`, treat its current `plan.md` and `problem.md` as required replanning context.
Read any `result.md`, relevant current canon, and index context as applicable.

When settled requirements or design changes invalidate an unstarted plan, keep the invalidated `plan.md` until replanning and record in `problem.md`:

- the earlier assumptions that mattered to the plan boundary;
- the settled change that invalidated those assumptions;
- the resulting impact on the plan boundary.

Keep this record causal and descriptive, and set the plan to `replan-required` in the same change.

When replanning, follow linked planning history only as far as needed to check for repeated invalid assumptions or boundaries.
For each relevant historical planning bundle, read its `plan.md` and `problem.md`, and read `result.md` when one exists and its execution result matters to the new boundary.
Treat current canon as authoritative and historical records as context rather than current constraints.

Preserve the superseded planning records that exist as history, create the replacement `plan.md`, and link it directly to the immediately preceding plan.
Use repository conventions for historical placement; otherwise keep each superseded planning bundle in a semantic subdirectory under `history/`.
Keep historical plans out of the active index and keep predecessor links traversable after archival.
Return the current plan to `planned` in the same change.

## Keep documentation navigable

When this skill adds, deletes, moves, or renames implementation-planning documents, update the relevant `index.md` in the same change.
Do not add links to documents that do not yet exist.

This skill defines planning semantics only.
Commit, push, pull-request, and merge workflows belong to separate tooling or skills.
