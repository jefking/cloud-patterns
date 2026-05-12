# CQRS Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the CQRS pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Separate read operations from write operations with distinct models, interfaces, and scaling or consistency choices.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Read and write workloads have different performance, scale, security, or model requirements.
- The domain needs command-side invariants that should not be exposed through generic CRUD updates.
- The team accepts added complexity from synchronization and eventual consistency.

Do not use this pattern when:
- Simple CRUD with one model is sufficient.
- Queries would mutate state or commands would bypass invariants.
- Separate stores are proposed without a real read-write mismatch.

## Architecture Invariants
- Commands express intent and are the only path for state-changing business operations.
- Queries return data and do not mutate domain state.
- Write models enforce invariants; read models optimize retrieval.
- Synchronization from writes to reads has explicit latency and failure behavior.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Version command contracts and read projections independently when needed.
- Expose projection lag, failed projection events, and rebuild status.
- Protect read stores from writes that bypass command validation.
- Define what clients see immediately after command success.

## Pattern Boundaries and Common Confusions
- CQRS does not require Event Sourcing, though they can be combined.
- Separate controllers alone are not CQRS if they use the same mutable CRUD model.
- Materialized View can implement the read side, but CQRS is the command-query split.

## Verification Checklist
- Can every state change be traced through a command handler?
- Are query handlers side-effect free?
- Are projection lag and rebuild paths tested?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
