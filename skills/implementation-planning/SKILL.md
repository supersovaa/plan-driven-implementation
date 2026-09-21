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
Do not treat an open PR, a closed-but-unmerged PR, or documents that exist only on another branch as completed work merely because the implementation exists there.

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
- permitted temporary implementation, when relevant;
- constraints that are already settled and materially affect implementation;
- what the implementer may decide autonomously;
- completion criteria.

Do not copy canonical requirements or design into the plan merely to make it self-contained.
Do not prescribe files, types, functions, algorithms, or implementation order unless those details are already settled constraints.
The plan defines what must be established and the boundary of the work; implementation mechanics normally belong to the implementer.

## Record dependencies centrally

Determine plan dependencies and record them in the nearest common `index.md` for the plans they relate to.
The index should make it possible to identify each plan, its durable state, and its direct dependencies, so implementable work can be derived from facts rather than stored as a separate `ready` flag.

Use only these durable plan states unless an existing repository convention provides an equivalent model:

- `planned`
- `completed`
- `replan-required`

Do not persist transient execution state such as `in-progress` or derived state such as `blocked`.

## Replanning

When a plan is `replan-required`, treat its `problem.md` as required input to replanning together with the current plan, result, relevant canon, and index context.

If the work unit still represents the same responsibility, replace the current `plan.md` rather than creating versioned plan files.
Delete obsolete `result.md` and `problem.md`, and return the state to `planned` in the same change.
Git history is the history of superseded plans and stopped attempts.

If the meaning of the work unit itself has changed, create a new semantic work unit instead of disguising a different responsibility as a revision of the old one.

## Keep documentation navigable

When this skill adds, deletes, moves, or renames implementation-planning documents, update the relevant `index.md` in the same change.
Do not add links to documents that do not yet exist.

This skill defines planning semantics only.
Commit, push, pull-request, and merge workflows belong to separate tooling or skills.
