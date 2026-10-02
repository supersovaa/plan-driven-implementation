---
name: requirement-driven-testing
description: Derive executable tests from settled requirements and design, prefer establishing missing coverage before implementation, distinguish requirement behavior from design contracts, and prioritize closing test gaps found in implementation review.
---

# Requirement-driven testing

Use this skill when implementing or changing behavior governed by settled requirements or design, and when implementation review finds missing or insufficient tests.

Treat tests as executable evidence of established requirements and design rather than as a source for inventing them.

## Keep requirement and design tests distinct

Derive requirement tests from settled requirements.
Use them to verify externally meaningful behavior, outcomes, constraints, and other requirement-level guarantees.

Derive design tests from settled design.
Use them when APIs, invariants, type guarantees, responsibility boundaries, or other design contracts need executable verification.

Requirements take precedence over design, tests, and implementation.
Do not preserve a design test or implementation behavior that conflicts with a settled requirement.
Leave requirement changes to the workflow that owns requirements.

Do not test implementation details merely because they exist.
Test an internal type, structure, or procedure directly only when it carries an established design guarantee.

## Reuse sufficient coverage

Before adding a test, inspect existing tests for the guarantee being established.

When an existing test already provides sufficient evidence for the requirement or design contract, use that evidence instead of adding a duplicate test.
Do not treat incidental execution of a path as sufficient coverage unless the test actually establishes the relevant guarantee.

## Establish tests before target implementation

When practical, add missing executable tests before implementing the behavior they establish.

For a new or changed requirement-level behavior:

1. Identify the smallest observable scenario that demonstrates the requirement.
2. Add or select the test that establishes that scenario.
3. Run it before the target implementation.
4. Confirm that it fails because the required behavior is not yet satisfied.
5. Implement the behavior.
6. Run the requirement test and relevant regression tests until they pass.

Apply the same ordering to design tests when an established design contract needs direct executable evidence.

A failure is useful pre-implementation evidence only when it is caused by the missing target behavior or contract.

## Distinguish test-support gaps from implementation dependencies

When a test cannot be expressed because test fixtures, builders, deterministic inputs, or other test-support boundaries are missing, establish the minimum reusable test support before the target implementation.

Do not use test support to fake a missing production dependency or predecessor behavior.
When the target scenario cannot meaningfully execute because required production behavior is not yet available, treat that as an implementation dependency and return it to the workflow that owns planning or execution readiness.

When the settled requirement does not determine an expected result, do not invent one in a test.

## Prioritize review-discovered test gaps

When implementation review finds that an established requirement or design contract lacks sufficient test evidence, close that test gap before ordinary cleanup or refactoring, except for prerequisites needed to build or run the tests.

When the implementation is incorrect, first add or strengthen a test that demonstrates the missing guarantee, then fix the implementation.

When the implementation is already correct and only the evidence is missing, a newly added review-remediation test may pass on its first run.

After closing the gap, use the resulting tests as regression protection for the remaining review fixes.

## Preserve repository conventions

Use the repository's established test framework, fixture structure, naming, placement, and traceability conventions.
Do not introduce a new documentation or mapping format when the repository already has one that can express the required relationship.

This skill owns deriving and restoring executable test evidence from settled requirements and design.
Requirement definition, design definition, implementation planning, implementation execution, and implementation review remain with their owning workflows.
