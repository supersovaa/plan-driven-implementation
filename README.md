# Implementation Workflow Skills

A lightweight three-skill workflow for planning implementation work, executing plans, and reviewing implementation results.

The skills are intentionally independent. They share a small file-level convention but do not require a shared protocol skill.

## Skills

- `implementation-planning`: turn already-settled requirements and design into small implementation plans.
- `plan-implementation`: implement one explicitly selected plan while preserving its boundary and recording its result.
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
    └── <superseded-boundary>/
        ├── plan.md
        ├── result.md     # present only when an execution result exists
        └── problem.md
```

Existing repository conventions take precedence when they express the same roles clearly.

Plan state is durable repository state, with these meanings:

- `planned`: the current plan is ready to be implemented when its dependencies are satisfied.
- `completed`: the current plan has been completed and its result recorded.
- `replan-required`: the current plan requires fresh planning because its boundary is no longer valid for continued implementation.

An invalidated unstarted plan remains current while it is `replan-required`; `problem.md` records the assumptions, invalidating change, and boundary impact needed for replanning.

A replacement plan links directly to the immediately preceding plan. Superseded planning bundles use semantic history paths, remain reachable through predecessor links, and stay outside the active index. Replanning may follow that chain to detect repeated invalid assumptions. Current canon remains authoritative, and the current plan defines the implementation boundary while its state is `planned`.

When replanning is directed from an implementation PR, finish that PR first as a coherent stopped attempt: record the replanning context, set the affected plan to `replan-required`, and keep unfinished implementation work outside the confirmed merged state. The replacement planning PR starts only after the repository workflow has merged that implementation PR. Once it is merged, the replanning instruction implicitly includes creating the planning PR through the repository workflow.

Unfinished implementation work may be preserved separately as working material. The planning PR starts from the updated confirmed base state, and any later reuse is checked against the replacement plan and that base state.

A minor plan correction that preserves the current plan boundary uses the opposite order. Create the planning correction through the repository workflow, then update the implementation PR to the new base and adjust its implementation only after that planning PR has been merged.

Each implementation PR implements one current plan. Another plan uses a separate implementation task and PR.

Derived transient states such as “ready”, “blocked”, or “in progress” do not need to be persisted.
