# Implementation Workflow Skills

A lightweight three-skill workflow for planning implementation work, executing one selected plan, and reviewing the result.

Each skill owns one phase.
Later phases treat the established outputs of earlier phases as inputs instead of re-performing the earlier phase.

## Skills

- `implementation-planning`: turn already-settled requirements and design into small implementation plans and replan invalidated boundaries.
- `plan-implementation`: execute one already-selected plan, preserve its boundary, record the result, and mark the plan completed or `replan-required`.
- `implementation-review`: review the implementation against the plan/result contract in addition to normal code review.

Planning validity belongs to `implementation-planning`.
`plan-implementation` reads and follows the selected plan without re-approving the planning work.
`implementation-review` remains an independent verification phase for the implementation outcome.

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

`problem.md` carries the causal context needed by later replanning.
A replacement plan links directly to the immediately preceding plan, while superseded planning bundles remain reachable as history.
Current canon remains authoritative.

Each implementation task and implementation PR covers one selected plan.
Transient states such as readiness, blocking, or active execution remain runtime concerns.
