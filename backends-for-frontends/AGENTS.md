# Backends for Frontends Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Backends for Frontends pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Create backend services dedicated to specific frontend experiences so each client gets an API shaped for its needs.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Different frontends have meaningfully different workflows, payload shapes, latency needs, or authentication context.
- A single generic backend forces clients to overfetch, underfetch, duplicate composition, or know internal topology.
- Frontend-specific behavior can be isolated without duplicating core business rules.

Do not use this pattern when:
- There is only one client type and no meaningful client-specific API needs.
- The backend would become a shared generic gateway for unrelated clients.
- The design duplicates domain rules across multiple BFFs without a shared service or policy boundary.

## Architecture Invariants
- Each frontend has a clearly owned backend contract and deployment lifecycle.
- The BFF composes, filters, and adapts data for that frontend only.
- Shared business capability remains behind shared services or libraries when it is not client-specific.
- Frontend clients do not call many internal services directly for the governed workflow.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Monitor latency, downstream fan-out, error rates, and payload sizes per BFF.
- Keep authentication, authorization, and tenant context explicit across BFF to service calls.
- Avoid unbounded aggregation; define timeouts and partial-response policy.
- Version each BFF contract around the frontend release cadence.

## Pattern Boundaries and Common Confusions
- Gateway Aggregation is about combining backend calls; BFF is about client-specific backend ownership.
- Gateway Routing exposes one endpoint for many services; BFF exposes a tailored backend for one frontend class.
- A BFF is not a place to copy all domain logic from downstream services.

## Verification Checklist
- Can each frontend evolve without forcing unrelated clients to change?
- Does the BFF reduce client complexity and internal coupling?
- Are shared and frontend-specific responsibilities separated in tests and code?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
