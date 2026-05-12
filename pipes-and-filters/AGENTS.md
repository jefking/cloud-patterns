# Pipes and Filters Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Pipes and Filters pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/pipes-and-filters
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://www.enterpriseintegrationpatterns.com/patterns/messaging/PipesAndFilters.html

## Pattern Intent
Break complex processing into independent filters connected by pipes with explicit input and output contracts.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A processing task is naturally a sequence of transformations, validations, enrichments, or routing steps.
- Steps should be reusable, independently testable, replaceable, or scalable.
- Intermediate contracts can be defined between steps.

Do not use this pattern when:
- All steps require hidden shared mutable state or tight temporal coupling.
- The whole task is simpler and safer as one cohesive operation.
- The pipeline order is accidental and undocumented.

## Architecture Invariants
- Each filter has one focused processing responsibility.
- Pipes carry data between filters through explicit contracts.
- Filters do not depend on downstream implementation details.
- Pipeline composition and ordering are visible in code or configuration.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define per-filter retry, error channel, poison-data behavior, and metrics.
- Make filters idempotent when they can be retried.
- Capture correlation and message history across the pipeline.
- Test filters individually and the composed pipeline end to end.

## Pattern Boundaries and Common Confusions
- Choreography coordinates services through events; Pipes and Filters structures a processing pipeline.
- Scheduler Agent Supervisor supervises distributed work; this pattern composes transformations.
- A long method split into private functions is not enough unless boundaries and contracts are explicit.

## Verification Checklist
- Can a filter be replaced or reordered without changing other filters when contracts allow it?
- Are invalid intermediate outputs caught at pipe boundaries?
- Can operators identify which filter failed or is slow?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
