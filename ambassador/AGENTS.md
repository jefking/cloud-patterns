# Ambassador Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Ambassador pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/ambassador
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Create a colocated helper service or proxy that sends outbound network requests on behalf of a consumer application.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A client needs outbound connectivity behavior that should be standardized outside the client process.
- The target concern is network communication: routing, authentication, TLS, retries, timeout policy, telemetry, or protocol adaptation.
- The helper can be deployed and operated next to, or on behalf of, the consuming workload.

Do not use this pattern when:
- The task is to expose one public ingress endpoint for many backends; use a gateway pattern instead.
- The helper would contain domain business logic or own the downstream data model.
- A simple in-process client library is sufficient and language-specific reuse is acceptable.

## Architecture Invariants
- The consumer owns business decisions; the ambassador owns outbound communication concerns.
- The consumer calls the ambassador boundary instead of directly calling the remote dependency.
- The ambassador is independently configurable, observable, health checked, and replaceable.
- Context that must cross the boundary, such as correlation IDs and identity, is explicit and validated.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define latency overhead and timeout budgets added by the proxy hop.
- Retry behavior must be operation-safe; require idempotency or an idempotency key before retrying mutations.
- Expose downstream health, circuit state, request counts, errors, and latency at the ambassador boundary.
- Package, deploy, and version the ambassador separately from the consumer when possible.

## Pattern Boundaries and Common Confusions
- Sidecar is a deployment style; Ambassador is specifically about outbound communication mediation.
- Gateway Routing handles ingress routing for clients; Ambassador handles egress from a consumer.
- Service mesh can be used as the implementation, but the pattern still requires an explicit outbound policy boundary.

## Verification Checklist
- Can the consumer perform its use case without knowing remote addresses, certificates, retry rules, or protocol quirks?
- Is every downstream call for the governed dependency forced through the ambassador?
- Are failure, retry, and observability behaviors tested at the ambassador boundary?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
