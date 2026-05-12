# Compute Resource Consolidation Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Compute Resource Consolidation pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/compute-resource-consolidation
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Run compatible tasks in a shared compute unit to improve utilization, reduce overhead, or simplify operations.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Multiple tasks have compatible resource profiles, security needs, and deployment lifecycles.
- Separate compute units create meaningful idle capacity, cost, or operational overhead.
- The consolidated unit can still expose per-task limits and observability.

Do not use this pattern when:
- Tasks have conflicting CPU, memory, latency, compliance, tenancy, or failure-isolation requirements.
- Independent deployment or rollback is required for each task.
- The design hides unrelated business capabilities in one opaque monolith.

## Architecture Invariants
- Consolidation has a stated resource or operational rationale.
- Each task remains logically identifiable, measurable, and controllable.
- Resource limits or scheduling rules prevent one task from starving others.
- Failure in one task does not silently corrupt or block unrelated tasks.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Measure utilization before and after consolidation.
- Define per-task health, logs, metrics, alerts, and restart behavior.
- Document shared scaling behavior and its effect on each task.
- Test contention, startup ordering, shutdown, and partial task failure.

## Pattern Boundaries and Common Confusions
- This pattern optimizes compute placement; it is not a license to merge domains.
- Bulkhead isolates resources; consolidation intentionally shares them with controls.
- Sidecar colocates a helper with an app; consolidation colocates multiple tasks in one compute unit.

## Verification Checklist
- Can operators identify which task is consuming resources?
- Does consolidation preserve required isolation and rollback behavior?
- Is cost or utilization improvement measurable?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
