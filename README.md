# Implementation Workflow Skills

A lightweight three-skill workflow for planning implementation work, executing one selected plan, and reviewing the result.

Each skill owns one phase.
Later phases treat established outputs of earlier phases as inputs instead of re-performing the earlier phase.

## Skills

- `plan-driven-planning`: turn already-settled requirements and design into small implementation plans and replan invalidated boundaries.
- `plan-driven-implementation`: execute one already-selected plan, preserve its boundary, record the result, and mark the plan `completed` or `replan-required`.
- `plan-driven-review`: review the implementation against the plan/result contract in addition to normal code review.

Planning validity belongs to `plan-driven-planning`.
`plan-driven-implementation` begins from a selected plan and established execution readiness.
`plan-driven-review` independently verifies the implementation outcome.

## Default document convention

When a repository has no established equivalent convention, use semantic work directories such as:

```text
docs/implementation/<work-name>/
├── index.md
├── plan.md
├── result.md     # created when an execution result exists
├── problem.md    # replanning context for replan-required work
└── history/      # superseded planning records, when needed
    └── <superseded-boundary>/
        ├── plan.md
        ├── result.md     # present only when an execution result exists
        └── problem.md
```

Existing repository conventions take precedence when they express the same roles clearly.

Plan state is durable repository state, with these meanings:

- `planned`: the current plan is available for implementation under the surrounding workflow.
- `completed`: the current plan passed its completion audit and its result is recorded.
- `replan-required`: the current plan boundary became invalid for continued implementation.

`problem.md` carries the causal context from an implementation attempt that invalidated the plan boundary.
A replacement plan links directly to the immediately preceding plan, while superseded planning bundles remain reachable as history.
Current canon remains authoritative.

Before implementation starts, planning-time discoveries update the active plan and index directly, including dependency, concurrency-constraint, ordering, placement, and boundary changes.
Once implementation starts, its active plan stays fixed for that attempt.
Implementation-time discoveries within the existing boundary belong to implementation and its recorded result.
`result.md` records what the implementation established directly rather than only as deviations from `plan.md`.

Direct dependencies express required predecessor results and gate implementation.
Merge prerequisites are separate planning metadata: a plan may be implemented and become `completed` while its result waits for a named repository-observable condition before incorporation.
Concurrency conflicts prevent simultaneous execution of independently completable plans without imposing an execution order or restricting otherwise-permitted implementation choices.
Merge eligibility is derived from merge prerequisites, so an unmet prerequisite does not create another durable plan state.
Transient states such as readiness, blocking, merge eligibility, or active execution remain runtime concerns.
