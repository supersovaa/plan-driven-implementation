# Implementation Workflow Skills

A lightweight implementation workflow with three phase-oriented skills and one testing discipline.

The phase-oriented skills plan implementation work, execute one selected plan, and review the result.
`requirement-driven-testing` derives executable evidence from settled requirements and design before implementation when practical and restores missing evidence found during review.

Each skill owns one coherent responsibility.
Later phases treat established outputs of earlier phases as inputs instead of re-performing the earlier phase.

## Skills

- `implementation-planning`: turn already-settled requirements and design into small implementation plans and replan invalidated boundaries.
- `requirement-driven-testing`: derive requirement and design tests from settled canon, establish missing coverage before target implementation when practical, and prioritize review-discovered test gaps.
- `plan-implementation`: execute one already-selected plan, preserve its boundary, record the result, and mark the plan `completed` or `replan-required`.
- `implementation-review`: review the implementation against the plan/result contract in addition to normal code review.

Planning validity belongs to `implementation-planning`.
`requirement-driven-testing` establishes executable evidence without redefining requirements or design.
`plan-implementation` begins from a selected plan and established execution readiness.
`implementation-review` independently verifies the implementation outcome.

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

Direct dependencies express required predecessor results. Concurrency conflicts are separate planning metadata: they prevent simultaneous execution of independently completable plans without imposing an execution order or restricting otherwise-permitted implementation choices.
Transient states such as readiness, blocking, or active execution remain runtime concerns.
