---
name: plan-driven-planning
description: Create, update, replace, and evaluate small, repository-persistent implementation plans from already-settled requirements and design while preserving those upstream decisions, including plan-validity review, while deferring the completeness audit of required test definitions to a pre-execution readiness gate, with explicit boundaries, dependencies, concurrency constraints, and durable state.
---

# Plan-Driven Planning

Use this skill after the relevant requirements and design decisions are settled, when creating, revising, replanning, or evaluating an implementation plan for validity.
Treat those upstream decisions as established inputs.
Project them into implementation work rather than reopening them during planning.

## Preserve settled design provenance

Project settled requirements and design into implementation work.
When they already support a coherent implementation plan, use them as-is.

Require every design-significant responsibility, boundary, structure, interface, and behavior decision reflected in the plan to be established by settled requirements or settled design.
Treat current repository responsibilities and behavior as baseline facts for mapping those established decisions onto implementation, not as authority to add or change design.
Derive planning decisions such as implementation boundaries, dependencies, concurrency constraints, and completion criteria from those established inputs and baseline facts.

When multiple design alternatives remain compatible with the established inputs, return the design choice to its owning workflow when it must be settled upstream, or leave implementation mechanics to the implementation phase when the plan does not need that choice.
Keep an already-workable settled design unchanged when a preferable alternative appears during planning.

When a coherent plan cannot be formed without adding or changing a design decision, identify that unresolved decision and return it to its owning workflow before continuing the dependent planning.

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
- completion criteria, including the validation obligations that must be satisfied for completion and links to required test definitions when already available.

Reference canonical requirements and design instead of copying them into the plan.
Prescribe files, types, functions, algorithms, or implementation order only when those details are already settled constraints.
The plan defines what must be established and the boundary of the work; implementation mechanics belong to the implementation phase.

## Review plans through owned deltas

When evaluating an implementation plan before execution, establish five linked views:

- the relevant repository baseline;
- the target behavior and responsibilities established by settled requirements and design;
- the implementation delta from that baseline to the target;
- the plan or stage that owns each part of that delta;
- the validation obligation that demonstrates the owned delta at completion.

Classify limitations observed in the current repository as baseline facts.
Evaluate plan quality by tracing every required delta to a concrete owner, completion criterion, and validation obligation.
A planning finding exists when that trace exposes an ownership, dependency, staging, or validation gap.
For staged work, evaluate temporary behavior against the stage that owns it and trace deferred full behavior to its planned owner.
Treat current repository examples, fixtures, and registered data as baseline context, and evaluate validation against the planned completion contract, including planned fixtures or test data when the contract requires them.

## Audit required test definitions before execution readiness

Required test definitions may be recorded during plan creation when they are already known.
Individual plan review does not audit whether every required test case and expected outcome has been recorded.

Before implementation begins, audit the plan's settled completion contract and derive every required test case and expected outcome, grounded in settled requirements and design.
When a coordinating workflow defines a later pre-execution gate, such as fixing a wave of plans for implementation, that gate owns this completeness audit.
Do not establish execution readiness until all required test definitions are recorded in repository-persistent documentation using repository conventions and linked as part of the plan's completion criteria.

These are planning-time definitions of the tests required for plan completion, not executable test code.
When requirements or design do not determine an expected outcome, return that ambiguity to its owning workflow.
Executable test implementation belongs to the testing workflow.

## Ground completion criteria in current responsibilities

Derive the current plan and its completion criteria from concrete responsibilities established by settled requirements and design that can be implemented and validated within that plan once its direct dependencies are satisfied.
When another concrete use case or implementation path overlaps with the current work, use a shared responsibility when settled requirements or design already establish it.
When satisfying both cases requires a new or generalized responsibility, return that design decision to its owning workflow before planning from it.

## Plan deferred stages explicitly

When a use case should flow end to end before one stage reaches its final implementation, decide that staged approach in the plan.
For each deferred stage, state:

- what behavior the temporary implementation must provide so the surrounding flow can be implemented and validated;
- what full behavior is deferred to later work.

Let the implementer choose the simplest temporary form that satisfies that contract.
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
Every prerequisite result required by a plan must either already be established or have a planned owner.
When an unestablished prerequisite result has no planned owner, assign its ownership using the normal implementation-boundary rules.
When that ownership belongs to a separate plan, record the dependent plan's direct dependency on that plan.

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

When a stopped attempt reveals a prerequisite result outside the invalidated boundary, replan the remaining work using the normal implementation-boundary and direct-dependency rules.
Propose the replacement plan structure produced by those rules.
Let the repository's work-number workflow assign replanned work according to its own rules.

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

This skill owns implementation planning, replanning, and plan-validity review semantics.
Execution and implementation-outcome review belong to their respective skills.
