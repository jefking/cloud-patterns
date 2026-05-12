# Health Endpoint Monitoring Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Health Endpoint Monitoring pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/health-endpoint-monitoring
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Expose endpoints that monitoring and automation can use to determine whether a workload is live, ready, and functioning.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- External monitors, orchestrators, or load balancers need a reliable health signal.
- The service has meaningful states such as liveness, readiness, startup, dependency health, or degraded mode.
- Automation can act on the reported state.

Do not use this pattern when:
- The endpoint is only a shallow ping but is used to prove business readiness.
- Health responses leak secrets, connection strings, or sensitive internal topology.
- Checks are so expensive that monitoring can harm the workload.

## Architecture Invariants
- Health endpoints are purpose-built and documented for their automation consumer.
- Liveness, readiness, startup, and dependency checks are separated when they mean different things.
- Health status maps to actionable states such as serve, drain, restart, alert, or degrade.
- Responses are safe, bounded, and do not expose sensitive detail.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Set check timeouts and budgets lower than monitoring intervals.
- Use dependency checks that prove required behavior without causing load or side effects.
- Log health transitions and expose metrics for each check component.
- Protect private health endpoints with network controls or authentication where appropriate.

## Pattern Boundaries and Common Confusions
- Health checks observe state; Circuit Breaker controls dependency call behavior.
- Readiness is not liveness; do not restart a process simply because a dependency is temporarily unavailable.
- A status page for humans is not a substitute for machine-actionable health endpoints.

## Verification Checklist
- Can a load balancer safely remove an unready instance?
- Can an orchestrator avoid restarting a live but not-ready process unnecessarily?
- Are degraded dependencies reflected without leaking sensitive internals?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
