# Implementation Workflow Skills

A lightweight three-skill workflow for planning implementation work, executing plans, and reviewing implementation results.

The skills are intentionally independent. They share a small file-level convention but do not require a shared protocol skill.

## Skills

- `implementation-planning`: turn already-settled requirements and design into small implementation plans.
- `plan-implementation`: implement explicitly selected plans while preserving plan boundaries and recording results.
- `implementation-review`: add plan/result-specific review responsibilities on top of normal code review.

## Default document convention

When a repository has no established equivalent convention, use semantic work directories such as:

```text
docs/implementation/<work-name>/
├── index.md
├── plan.md
├── result.md     # created when an execution result exists
├── problem.md    # replanning context for replan-required work
└── history/      # superseded planning records, when needed
```

Existing repository conventions take precedence when they express the same roles clearly.

Plan state is durable repository state, with these meanings:

- `planned`: the current plan is ready to be implemented when its dependencies are satisfied.
- `completed`: the current plan has been completed and its result recorded.
- `replan-required`: the work unit requires fresh planning because its plan boundary became stale or an implementation attempt stopped at that boundary.

An invalidated unstarted plan remains current while the work unit is `replan-required`; `problem.md` records the assumptions, invalidating change, and boundary impact needed for replanning.

A replacement plan links directly to the immediately preceding plan. Superseded planning records remain reachable as history and stay outside the active index. Replanning may follow that predecessor chain to detect repeated invalid assumptions; current canon and the current plan remain authoritative for implementation.

Derived transient states such as “ready”, “blocked”, or “in progress” do not need to be persisted.
