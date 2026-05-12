# Gateway Routing Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Gateway Routing pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-routing
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Expose one client endpoint and route requests to multiple backend services by path, host, version, tenant, or policy.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Clients should not know internal service locations or topology.
- A single public endpoint can route to multiple backend services.
- Routing policy can be expressed with stable rules and health-aware behavior.

Do not use this pattern when:
- The main behavior is combining multiple backend responses.
- Clients still call internal service endpoints directly.
- Routing decisions require domain workflow orchestration rather than request routing.

## Architecture Invariants
- The gateway owns the public endpoint and route map.
- Each request is routed to the appropriate backend based on explicit policy.
- Internal backend addresses remain hidden from clients.
- Correlation, identity, headers, and request metadata are preserved or intentionally transformed.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Support version routing, tenant routing, canary routing, and health-aware failover when relevant.
- Return clear errors for missing routes, unauthorized routes, and unavailable backends.
- Keep route changes auditable and testable.
- Monitor route hit counts, backend failures, and routing latency.

## Pattern Boundaries and Common Confusions
- Gateway Aggregation makes multiple downstream calls; Gateway Routing chooses one route per request.
- Backends for Frontends creates client-specific APIs; routing can sit in front of many APIs.
- A load balancer is not enough unless it expresses the required service routing policy.

## Verification Checklist
- Can backend locations change without client changes?
- Are route selection and failure cases covered by tests?
- Can unhealthy backends be removed from routing?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
