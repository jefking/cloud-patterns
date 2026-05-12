# Gateway Offloading Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Gateway Offloading pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/gateway-offloading
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Move shared edge concerns to a gateway so backend services do not duplicate specialized request processing.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Multiple backend services need the same cross-cutting edge behavior.
- The concern can be enforced before or after forwarding without domain knowledge.
- Centralized gateway policy improves consistency, security, reliability, or operations.

Do not use this pattern when:
- The behavior is core business authorization or domain decision making.
- Each service needs materially different logic that cannot be generalized safely.
- The gateway becomes an opaque application layer with hidden business state.

## Architecture Invariants
- The offloaded responsibility is named and enforced at the gateway boundary.
- Backends no longer need to duplicate that responsibility.
- Identity, correlation, client IP, and policy decisions are forwarded safely when needed.
- Gateway behavior is configured, versioned, tested, and observable.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Common offloads include TLS termination, authentication prechecks, compression, caching, rate enforcement, WAF, and protocol translation.
- Treat gateway outage and misconfiguration as high-blast-radius risks.
- Audit policy changes and test them before production rollout.
- Avoid adding latency-heavy work to every request unless justified.

## Pattern Boundaries and Common Confusions
- Gateway Routing routes requests; Gateway Offloading performs shared request processing.
- Gateway Aggregation composes responses; Gateway Offloading removes repeated cross-cutting work.
- Service mesh may implement offloading, but the offloaded responsibility must still be explicit.

## Verification Checklist
- Can a backend remove duplicated edge code and still satisfy the policy?
- Are gateway decisions observable and auditable?
- Do tests prove protected requests fail before reaching backend services?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
