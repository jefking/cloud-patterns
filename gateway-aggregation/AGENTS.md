# Gateway Aggregation Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Gateway Aggregation pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-aggregation
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Use a gateway endpoint to call multiple backend services and return one client-oriented aggregated response.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A client would otherwise make several backend calls for one user action or screen.
- Client chattiness, network latency, or backend topology leakage is a problem.
- The gateway can enforce composition policy, timeouts, and partial failure behavior.

Do not use this pattern when:
- The gateway only routes one request to one backend without aggregation.
- Aggregation would hide critical backend failures as successful business outcomes.
- The gateway would become the authoritative domain store.

## Architecture Invariants
- The client calls one aggregate endpoint for a defined aggregate use case.
- The gateway performs multiple downstream calls and shapes one response contract.
- Downstream timeout, retry, fallback, and partial-response policy are explicit.
- The aggregate response does not expose internal service topology unnecessarily.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Use fan-out budgets so one aggregate request cannot create unbounded downstream load.
- Trace each downstream call under one correlation ID.
- Cache or parallelize only when freshness and dependency limits allow it.
- Monitor aggregate latency by downstream contribution.

## Pattern Boundaries and Common Confusions
- Gateway Routing selects a backend; Gateway Aggregation combines backend responses.
- Backends for Frontends owns a client-specific backend; aggregation can be one behavior inside a BFF.
- API composition is not domain ownership.

## Verification Checklist
- Does the client make fewer calls for the same workflow?
- Are partial failures represented according to contract?
- Can a backend topology change without changing the client response shape?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
