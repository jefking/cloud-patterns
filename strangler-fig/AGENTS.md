# Strangler Fig Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Strangler Fig pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Incrementally replace a legacy system by routing selected capabilities to new implementations while both coexist.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A legacy system must be modernized with lower risk than a big-bang replacement.
- Capabilities can be sliced, intercepted, routed, or wrapped one at a time.
- The system can manage coexistence, data ownership, rollback, and cutover.

Do not use this pattern when:
- The plan is to rewrite everything before any production traffic moves.
- There is no routing, facade, proxy, or interception point to control migration.
- New and old systems would create uncontrolled duplicate sources of truth.

## Architecture Invariants
- A facade, router, proxy, or interception layer controls whether traffic goes to legacy or new behavior.
- Migration slices are small enough to test, release, roll back, and observe independently.
- Data ownership and synchronization are explicit for each slice.
- Legacy bypass paths are removed or controlled after a slice is migrated.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Track traffic split, migrated capability inventory, error rates, parity checks, and rollback readiness.
- Use feature flags, route rules, shadow traffic, or adapters when appropriate.
- Define retirement criteria for legacy code and data after migration.
- Test legacy route, new route, rollback, and coexistence for every slice.

## Pattern Boundaries and Common Confusions
- Anti-Corruption Layer protects a model boundary; Strangler Fig replaces capabilities over time.
- Gateway Routing can implement traffic switching but is not the whole migration pattern.
- Parallel new implementation is not enough unless production traffic can be incrementally moved.

## Verification Checklist
- Can one capability be moved without moving the whole system?
- Can traffic be rolled back to legacy safely?
- Is there a plan to retire each legacy slice after cutover?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
