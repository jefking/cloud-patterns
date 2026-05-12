# Sidecar Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Sidecar pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Deploy a helper component beside an application process or container to provide supporting capabilities without embedding them in the app.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A supporting concern should share the application's local environment and lifecycle.
- The helper can be packaged, configured, or updated separately from business code.
- Local communication between app and helper is acceptable and observable.

Do not use this pattern when:
- The helper is a centralized shared service for many unrelated applications.
- The helper owns core domain business behavior.
- Lifecycle coupling would make availability, scaling, or resource use worse than embedding or centralizing.

## Architecture Invariants
- The primary application and sidecar are clearly identified.
- The sidecar is colocated and lifecycle-coupled with the app instance.
- Responsibilities are limited to supporting capabilities such as proxying, logging, config sync, security, or telemetry.
- Resource limits, startup order, shutdown, and health are defined for both components.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Monitor sidecar resource consumption and failure separately from the app.
- Define what the app does when the sidecar is absent, starting, degraded, or failed.
- Keep local APIs between app and sidecar stable and minimal.
- Test deployment rollouts and version compatibility between app and sidecar.

## Pattern Boundaries and Common Confusions
- Ambassador can be implemented as a sidecar, but Ambassador is specifically outbound network mediation.
- Gateway patterns sit at shared ingress; Sidecar sits beside each application instance.
- Compute Resource Consolidation shares compute for tasks; Sidecar attaches a helper to one primary app.

## Verification Checklist
- Is the helper colocated with each app instance that uses it?
- Can the app's business logic remain independent of helper implementation details?
- Are health checks and resource limits defined for the sidecar?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
