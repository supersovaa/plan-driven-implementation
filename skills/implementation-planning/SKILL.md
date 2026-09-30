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
- permitted temporary implementation, when relevant;
- constraints that are already settled and materially affect implementation;
- what the implementer may decide autonomously;
- completion criteria.

Do not copy canonical requirements or design into the plan merely to make it self-contained.
Do not prescribe files, types, functions, algorithms, or implementation order unless those details are already settled constraints.
The plan defines what must be established and the boundary of the work; implementation mechanics normally belong to the implementer.

## Record dependencies centrally

Determine plan dependencies and record them in the nearest common `index.md` for the current work units they relate to.
The index should make it possible to identify each current work unit, its durable state, and its direct dependencies, so implementable work can be derived from facts rather than stored as a separate `ready` flag.
Superseded plans and retired work units are planning history rather than active index entries.

Use only these durable plan states unless an existing repository convention provides an equivalent model:

- `planned`
- `completed`
- `replan-required`

Do not persist transient execution state such as `in-progress` or derived state such as `blocked`.

## Replanning

When a work unit is `replan-required`, treat its current `plan.md` and `problem.md` as required replanning context.
Read any `result.md`, relevant current canon, and index context as applicable.
Treat current canon as authoritative; planning history explains earlier boundaries and failures without overriding current decisions.

When settled requirements or design changes invalidate an unstarted plan, keep the invalidated `plan.md` in place until replanning and record in `problem.md`:

- the earlier assumptions that mattered to the plan boundary;
- the settled change that invalidated those assumptions;
- the resulting impact on the plan boundary.

Keep this record causal and descriptive, and set the work unit to `replan-required` in the same change.

When replanning, follow any linked predecessor plans only as far as needed to check whether the new plan repeats an earlier invalidated assumption or boundary.
Historical plans, problems, and results are context for replanning rather than current constraints.

If the work unit still represents the same responsibility, preserve the superseded `plan.md`, `problem.md`, and any stopped-attempt `result.md` as planning history.
Create the new current `plan.md`, link it directly to the immediately preceding plan, and return the work unit to `planned` in the same change.
Use repository conventions for historical placement; otherwise keep superseded planning records under a `history/` location within the work unit.
Keep historical plans out of the active index.

If the responsibility moves to a different semantic work unit, create the successor work unit and link its new `plan.md` directly to the superseded plan.
Retire the old work unit from the active index while keeping its planning records reachable through that link.
Re-evaluate dependencies against the successor work unit instead of carrying them over mechanically.

A chain of direct predecessor links provides deeper history when repeated replanning makes it relevant.
Normal implementation uses current canon and the current plan; historical planning records are consulted when replanning requires them.

## Keep documentation navigable

When this skill adds, deletes, moves, or renames implementation-planning documents, update the relevant `index.md` in the same change.
Do not add links to documents that do not yet exist.

This skill defines planning semantics only.
Commit, push, pull-request, and merge workflows belong to separate tooling or skills.
