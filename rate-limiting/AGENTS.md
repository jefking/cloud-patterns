# Rate Limiting Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Rate Limiting pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/rate-limiting-pattern
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Control the rate at which a workload consumes a constrained dependency to avoid provider throttling, quota exhaustion, or retry storms.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- The workload calls a dependency with known quotas, limits, costs, or concurrency constraints.
- Caller-side demand shaping can prevent avoidable throttling or overload.
- The system can queue, delay, shed, or prioritize excess outbound work.

Do not use this pattern when:
- The service being called should enforce limits on incoming callers; use Throttling for that side.
- The design sends unlimited traffic and relies on provider errors for control.
- Retries ignore retry-after values or quota recovery windows.

## Architecture Invariants
- Limits are enforced before requests are sent to the constrained dependency.
- Budgets are scoped by dependency, operation, tenant, user, or priority as appropriate.
- Excess work has explicit behavior: wait, queue, reject, degrade, or reschedule.
- Retry and backoff policies respect the same rate budget.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Use token bucket, leaky bucket, concurrency semaphore, adaptive limiter, or provider SDK controls as appropriate.
- Track allowed, delayed, rejected, retried, and provider-throttled calls.
- Honor provider retry-after headers and dynamic quota signals.
- Prevent distributed instances from independently exceeding a shared global quota.

## Pattern Boundaries and Common Confusions
- Rate Limiting is caller-side consumption control; Throttling is service-side protection from callers.
- Circuit Breaker responds to unhealthy dependencies; Rate Limiting respects normal limits even when healthy.
- Bulkhead reserves resource pools; Rate Limiting controls flow rate.

## Verification Checklist
- Can the workload stay below documented dependency limits under load?
- Are retries counted against the same budget?
- Can operators see when work is delayed or rejected by the limiter?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
