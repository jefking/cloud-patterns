# Asynchronous Request-Reply Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Asynchronous Request-Reply pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/asynchronous-request-reply
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Accept a request quickly, process it asynchronously, and let the caller monitor status until a final result is available.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- The caller needs a timely acknowledgement but the work might exceed the request timeout or latency budget.
- The caller can poll, follow a status resource, or otherwise retrieve completion later.
- The system can persist status and correlate the request with background processing.

Do not use this pattern when:
- The caller requires immediate completion in the initial response.
- The interaction is continuous streaming or server push rather than request, status, result.
- The system cannot reliably persist status, result, and failure state.

## Architecture Invariants
- The acceptor validates the request before returning an accepted response.
- The response includes a stable status reference, correlation ID, and polling guidance.
- The worker is separate from the initial request path and can recover after restart.
- Status states are explicit, including pending, running, succeeded, failed, canceled, and expired when relevant.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Use idempotency keys or equivalent duplicate-submission protection.
- Store terminal results and errors long enough for clients to observe them.
- Protect the status endpoint from hot polling with retry-after, backoff, caching, or quotas.
- Define cancellation, expiration, cleanup, and authorization for status resources.

## Pattern Boundaries and Common Confusions
- Queue-Based Load Leveling buffers work; Asynchronous Request-Reply defines the client-facing status contract.
- Publisher-Subscriber announces events; this pattern keeps a specific caller tied to a specific request outcome.
- Returning 202 without durable status tracking is not this pattern.

## Verification Checklist
- Can a client recover the result after losing the original connection?
- Are duplicate submissions safe?
- Do tests cover accepted, invalid, running, failed, canceled, and successful requests?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
