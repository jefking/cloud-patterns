# Bulkhead Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Bulkhead pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/bulkhead
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Isolate workload elements into separate resource pools so failure or saturation in one pool does not exhaust the rest.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A dependency, tenant, consumer class, feature, or worker group can overload shared resources.
- The system needs failure containment, priority isolation, or different quality-of-service levels.
- Resources can be partitioned through processes, pools, queues, compute instances, quotas, or platform limits.

Do not use this pattern when:
- The task requires maximum resource efficiency and accepts shared-fate failure.
- There is no credible resource exhaustion, noisy-neighbor, or cascading-failure risk.
- The design creates isolation labels but still uses one shared bottleneck for all traffic.

## Architecture Invariants
- Every bulkhead has an explicit boundary, capacity limit, owner, and failure mode.
- Work in one bulkhead cannot consume the resources reserved for another bulkhead.
- Fallback paths do not bypass isolation and recreate shared fate.
- Bulkhead health and saturation are measured independently.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define queue limits, connection limits, CPU and memory limits, concurrency caps, or tenant allocations.
- Use backpressure, rejection, or graceful degradation when a bulkhead is saturated.
- Combine with Retry, Circuit Breaker, or Throttling only within an overall resource budget.
- Test one bulkhead under failure while adjacent bulkheads continue to serve traffic.

## Pattern Boundaries and Common Confusions
- Sharding partitions data; Bulkhead partitions failure and resource consumption.
- Deployment Stamps duplicate full deployment units; Bulkheads can be smaller resource compartments.
- Throttling limits incoming consumption; Bulkhead reserves isolated capacity.

## Verification Checklist
- Can one overloaded tenant or dependency fail without exhausting unrelated work?
- Are per-bulkhead limits enforced by code, runtime, or infrastructure?
- Do dashboards show which bulkhead is saturated and why?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
