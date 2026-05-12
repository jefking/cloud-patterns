# Retry Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Retry pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/retry
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Retry operations that fail because of anticipated transient faults, using bounded and delay-aware policies.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- The failure is likely temporary, such as timeout, transient network error, throttling with recovery guidance, or temporary service unavailability.
- The operation is idempotent or protected from duplicate side effects.
- The caller has enough deadline budget for another attempt.

Do not use this pattern when:
- The error is validation, authorization, conflict, not found, or another deterministic business result.
- The operation is non-idempotent and has no duplicate protection.
- Retries would exceed caller deadlines, overload a dependency, or stack with other retry layers.

## Architecture Invariants
- Retryable and non-retryable errors are explicitly classified.
- Attempts are bounded by count, time, deadline, and cancellation.
- Delay uses exponential backoff, jitter, provider guidance, or another deliberate schedule.
- Final failure is surfaced clearly after retries are exhausted.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Track attempt count, delay, error class, final outcome, and exhausted retries.
- Use idempotency keys for mutations when response loss could hide success.
- Coordinate retry budgets across SDK, client, gateway, and application layers.
- Combine with Circuit Breaker when persistent failure should stop attempts temporarily.

## Pattern Boundaries and Common Confusions
- Retry handles transient failure; Circuit Breaker handles suspected persistent failure.
- Rate Limiting prevents quota exhaustion; retries must not defeat it.
- Retrying asynchronous messages requires idempotent consumers and dead-letter behavior, not just a loop.

## Verification Checklist
- Do tests prove terminal errors are not retried?
- Can duplicate attempts not create duplicate business effects?
- Is total retry time within the caller's deadline?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
