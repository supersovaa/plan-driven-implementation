---
name: implementation-planning
description: Create, update, or replace small, repository-persistent implementation plans from already-settled requirements and design, with explicit boundaries, dependencies, and durable state.
---

# Implementation Planning

Use this skill after the relevant requirements and design decisions are settled.
Treat those upstream decisions as established inputs.

## Establish the repository state first

Before choosing the next work, inspect the latest state of the explicitly relevant base branch.
Treat that branch as the source of confirmed plan state.

Use repository conventions when they already define where implementation plans live and how they are indexed.
Otherwise, prefer semantic work directories under `docs/implementation/`.
Prefer semantic identity over dates or sequence numbers for work units.

## Consider prerequisite refactoring

Before defining a new implementation boundary, inspect how the planned behavior fits responsibilities already present in the current repository state.
Use settled requirements, design, and repository state to determine whether the current structure supports a coherent, independently completable boundary and keeps behavior shared with existing paths under one coherent responsibility.
When it does, plan the current change against the current structure and treat related reorganization as optional improvement.
When establishing that boundary requires reorganizing existing responsibilities, represent the settled refactoring as prerequisite work and define the dependent implementation boundary from the resulting structure.
When prerequisite refactoring depends on a new design decision, return that decision to its owning workflow before planning the dependent implementation.

## Plan small implementation boundaries

Prefer work units that:

- have one clear implementation purpose;
- can be completed and validated independently once their direct dependencies are satisfied;
- are small enough that replanning does not require preserving a large partial implementation;
- keep future work outside the current scope until it is needed.

Split work by independent implementation boundaries rather than file count or diff size.
Create a `plan.md` only for a node with an independent implementation boundary.
Pure organizational directories may have only an `index.md`.

A parent may have its own `plan.md` when it has an independent integration-level objective and completion criteria beyond containing child plans.

## Keep plans about the implementation contract

A plan should define, in whatever structure fits the repository:

- the purpose of the work;
- references to the relevant requirement and design canon;
- implementation scope;
- out-of-scope work;
- the required behavior of temporary implementation, when a stage will be completed later;
- settled constraints that materially affect implementation;
- what the implementer may decide autonomously;
- completion criteria.

Reference canonical requirements and design instead of copying them into the plan.
Prescribe files, types, functions, algorithms, or implementation order only when those details are already settled constraints.
The plan defines what must be established and the boundary of the work; implementation mechanics belong to the implementation phase.

## Plan deferred stages explicitly

When a use case should flow end to end before one stage reaches its final implementation, decide that staged approach in the plan.
For each deferred stage, state:

- what behavior the temporary implementation must provide so the surrounding flow can be implemented and validated;
- what full behavior is deferred to later work.

Let the implementer choose the simplest temporary form that satisfies that contract.
When a particular temporary form is already a settled constraint, record it with the other implementation constraints.
Place the deferred full behavior in separate follow-up work when it is settled enough to plan.

## Derive dependencies from intended use

For each plan, derive dependencies from the conditions required for its intended use cases and completion criteria to hold in the repository.
When another plan's established result must exist first for the intended behavior to be exercised or validated, record that plan as a dependency.
Treat implementation scope and dependency as separate questions: work may stay outside the current plan's scope while its established result remains a prerequisite.
Judge dependencies from the intended repository use cases, even when isolated implementation or tests can supply artificial setup.

## Record dependencies centrally

Determine plan dependencies and record them in the nearest common `index.md` for the plans they relate to.
The index should make it possible to identify each current plan, its durable state, and its direct dependencies.

Use only these durable plan states unless an existing repository convention provides an equivalent model:

- `planned`
- `completed`
- `replan-required`

Transient execution state and derived readiness remain runtime concerns.

## Replanning

Before implementation starts, incorporate planning-time discoveries directly into the active plan and its index.
Dependency changes, ordering changes, document moves, and boundary adjustments found before execution are ordinary planning updates and keep the plan `planned`.

Reserve `replan-required` and `problem.md` for a plan boundary invalidated after an implementation attempt begins.

When planning is invoked for a `replan-required` plan, treat its current `plan.md` and `problem.md` as required context.
Read any `result.md`, relevant current canon, and index context as applicable.

Follow linked planning history only as far as needed to detect repeated invalid assumptions or boundaries.
For each relevant historical planning bundle, read its `plan.md` and `problem.md`, and read `result.md` when its execution result matters to the new boundary.
Treat current canon as authoritative and historical records as context.

Preserve superseded planning records as history, create the replacement `plan.md`, and link it directly to the immediately preceding plan.
Use repository conventions for historical placement; otherwise keep each superseded planning bundle in a semantic subdirectory under `history/`.
Keep historical plans outside the active index and keep predecessor links traversable after archival.
Return the current plan to `planned` in the same change.

## Keep documentation navigable

When this skill adds, deletes, moves, or renames implementation-planning documents, update the relevant `index.md` in the same change.
Add links when their target documents exist.

This skill owns implementation planning and replanning semantics.
Execution and review belong to their respective skills.
