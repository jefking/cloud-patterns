# Event Sourcing Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Event Sourcing pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Store state changes as an immutable append-only stream of domain events and derive current state from that stream.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A reliable history of business changes is essential for audit, reconstruction, temporal queries, or complex workflows.
- The domain can express state changes as meaningful events.
- The team can handle event versioning, replay, projection, and eventual consistency.

Do not use this pattern when:
- A mutable current-state table is the real source of truth.
- Events are only technical logs that cannot rebuild domain state.
- The system needs simple CRUD and the event model adds no business value.

## Architecture Invariants
- The event stream is the durable source of truth.
- Events are immutable, append-only, ordered within their stream, and represent business facts.
- Current state is rehydrated from events and optional snapshots.
- Read models and projections are derived and rebuildable.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define optimistic concurrency, stream identity, snapshots, schema evolution, and upcasting.
- Make event publication to external consumers reliable without losing events.
- Protect personally sensitive or erasable data with careful event design and retention policy.
- Monitor projection lag, replay failures, stream conflicts, and poison events.

## Pattern Boundaries and Common Confusions
- Event Sourcing is not just publishing events after database writes.
- CQRS can use Event Sourcing but does not require it.
- A change-data-capture log is not a domain event stream unless it carries business meaning and rebuild rules.

## Verification Checklist
- Can current aggregate state be rebuilt from events alone?
- Are event schema changes backward compatible or upcastable?
- Can projections be deleted and rebuilt safely?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
