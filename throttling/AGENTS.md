# Throttling Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Throttling pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/throttling
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Protect a service or resource by limiting the rate, concurrency, or volume of incoming consumption.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A service must protect itself from overload, tenant abuse, noisy neighbors, or resource exhaustion.
- Limits can be defined by caller, tenant, user, operation, resource, or plan.
- Callers can receive clear signals when they are over limit.

Do not use this pattern when:
- The problem is controlling this workload's outbound calls to a dependency; use Rate Limiting.
- Limits are enforced only after expensive protected work already ran.
- The service silently drops work without a clear contract.

## Architecture Invariants
- Limits are enforced before protected resources are exhausted.
- Limit scopes and counters are deterministic and resistant to spoofing.
- Over-limit responses are explicit and include retry guidance when appropriate.
- Throttling decisions are observable and auditable.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Use token bucket, leaky bucket, fixed window, sliding window, concurrency caps, or platform policies as appropriate.
- Track allowed, rejected, delayed, throttled, and near-limit requests by scope.
- Design fairness across tenants and priority classes.
- Test bursts, sustained overload, counter reset, distributed counters, and recovery.

## Pattern Boundaries and Common Confusions
- Throttling is service-side protection; Rate Limiting is caller-side consumption control.
- Bulkhead isolates capacity pools; Throttling limits access to capacity.
- Priority Queue chooses work order; Throttling controls admission.

## Verification Checklist
- Can one caller exceed its limit without affecting others?
- Are limits enforced early enough to protect scarce resources?
- Do callers receive consistent over-limit status and retry information?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
