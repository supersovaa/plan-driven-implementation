---
name: plan-driven-planning
description: Create, update, or replace small, repository-persistent implementation plans from already-settled requirements and design, distinguishing durable guarantees from provisional results and defining completion-required tests only for durable guarantees, with explicit boundaries, dependencies, concurrency constraints, and durable state.
---

# Plan-Driven Planning

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
- completion criteria, distinguishing durable guarantees from provisional or intermediate results and linking required test definitions for each durable guarantee.

Reference canonical requirements and design instead of copying them into the plan.
Prescribe files, types, functions, algorithms, or implementation order only when those details are already settled constraints.
The plan defines what must be established and the boundary of the work; implementation mechanics belong to the implementation phase.

## Separate completion from durable guarantees

Plan completion means the selected implementation boundary has been established and sufficiently validated.
It does not by itself make every completed behavior a permanent regression guarantee.

Within the completion criteria, distinguish:

- durable guarantees: behavior or contracts this plan establishes as expected to remain protected after completion;
- provisional or intermediate results: behavior needed to complete the current boundary while later work may replace, refine, or supersede it.

Require retained executable test evidence as a completion condition for durable guarantees.
Provisional or intermediate results still require enough validation to judge the current plan complete, but complete formal regression coverage is not inherently completion-required.
Do not classify an outcome as durable merely because it is observable or because the plan will be marked `completed`.
Derive durability from settled requirements, design, or explicit planning intent.

## Define required tests for durable guarantees before finalizing the plan

Before finalizing a plan, derive every test case and expected outcome required to establish the plan's durable guarantees, grounded in settled requirements and design.
Do not treat the plan as finalized until all such durable required test definitions are recorded in repository-persistent documentation using repository conventions and linked as part of the plan's completion criteria.
These are planning-time definitions of the executable evidence required for durable guarantees, not executable test code.

For provisional or intermediate results, record validation constraints or useful test candidates when they materially affect implementation, but do not require a complete formal test definition set merely to finalize the plan.
When requirements or design do not determine an expected outcome for a durable guarantee, return that ambiguity to its owning workflow.
Executable test implementation belongs to the testing workflow.

## Ground completion criteria in current responsibilities

Derive the current plan and its completion criteria from concrete responsibilities that can be established and validated within that plan once its direct dependencies are satisfied.
When another concrete use case or implementation path introduces overlapping responsibilities, extract the responsibility they actually share and generalize only as far as those concrete cases require.

## Plan deferred stages explicitly

When a use case should flow end to end before one stage reaches its final implementation, decide that staged approach in the plan.
For each deferred stage, state:

- what behavior the temporary implementation must provide so the surrounding flow can be implemented and validated;
- what full behavior is deferred to later work.

Let the implementer choose the simplest temporary form that satisfies that contract.
Unless the plan explicitly establishes otherwise, behavior provided only as a temporary implementation for a deferred stage is provisional rather than a durable guarantee.
It still requires enough validation to show that the current plan boundary works, but retained formal regression coverage is not completion-required solely because the temporary behavior exists.
When a particular temporary form is already a settled constraint, record it with the other implementation constraints.
Place the deferred full behavior in separate follow-up work when it is settled enough to plan.

When the user explicitly decides to defer responsibility from the current plan to later work, treat that decision as one planning transfer.
Do not trigger this transfer merely because later work appears possible or preferable during planning.
When the destination is settled enough to plan, update or replan the source plan according to its current state and the planning and replanning rules below, create or update the destination plan, and synchronize the relevant index, direct dependencies, and concurrency constraints in the same coherent change.
In a pull-request workflow, keep those transfer edits in the same pull request.
Do not leave transferred responsibility unowned or ambiguously owned by both plans.
If the user has decided to defer the responsibility but the destination cannot yet be planned because it depends on a new requirement or design decision, record the deferred responsibility and return that decision to its owning workflow instead of inventing a follow-up plan.

## Record dependencies and concurrency constraints centrally

Determine direct plan dependencies and record them in the nearest common `index.md` for the plans they relate to.
A direct dependency means that one plan requires the established result of another plan before it can be implemented.

Also determine known concurrency constraints between plans and record them in the nearest common `index.md`.
When two plans remain independently completable but either may change shared implementation in a way that can invalidate the other's implementation assumptions or overlap with its changes, record a concurrency conflict rather than an artificial dependency.
A concurrency conflict prevents the related plans from executing simultaneously; it does not impose an execution order or make either plan depend on the other's result.
Record the conflict symmetrically or in another repository convention that makes the mutual exclusion unambiguous.

The index should make it possible to identify each current plan, its durable state, its direct dependencies, and its concurrency conflicts.

When upstream planning uses concrete user-facing use cases as its primary units, structure the index with a separate plan table for each use case rather than one table ordered primarily by plan identifier or sequence number.
Treat plan identifiers and sequence numbers as plan attributes, not as the primary document-grouping axis.
If one plan is relevant to multiple use cases, it may be referenced from each relevant use-case table.
Keep mutable plan metadata, especially durable state, authoritative in one canonical entry and use references from the other use-case tables instead of duplicating that state.

Use only these durable plan states unless an existing repository convention provides an equivalent model:

- `planned`
- `completed`
- `replan-required`

Transient execution state and derived readiness remain runtime concerns.

## Replanning

Before implementation starts, incorporate planning-time discoveries directly into the active plan and its index.
Dependency changes, concurrency-constraint changes, ordering changes, document moves, and boundary adjustments found before execution are ordinary planning updates and keep the plan `planned`.

During planning updates and replanning, use discoveries to revise the planning decisions they invalidate while preserving implementation choices left open by settled constraints.
When a valid replacement plan depends on a new requirement or design decision, return that decision to its owning workflow before planning from it.

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

When this skill adds, deletes, moves, or renames implementation planning documents, update the relevant `index.md` in the same change.
Add links when their target documents exist.

This skill owns implementation planning and replanning semantics.
Execution and review belong to their respective skills.
