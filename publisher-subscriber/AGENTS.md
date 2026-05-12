# Publisher-Subscriber Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Publisher-Subscriber pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html

## Pattern Intent
Let publishers announce events asynchronously to multiple independent subscribers without knowing who receives them.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- One event should be available to zero, one, or many independent subscribers.
- Publishers should not call subscribers directly or know subscriber count.
- Subscribers can own their own processing, retries, and failure handling.

Do not use this pattern when:
- A command must be handled by exactly one receiver.
- The publisher needs an immediate response from every receiver.
- The design creates direct publisher-to-subscriber dependencies.

## Architecture Invariants
- Publishers emit immutable events to a broker, topic, or event bus.
- Subscribers register interest independently and receive their own copy or subscription stream.
- Event contracts are versioned and owned.
- One subscriber failure does not block unrelated subscribers.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define delivery guarantees, subscription durability, filtering, dead-lettering, and replay behavior.
- Make subscriber handlers idempotent and duplicate-safe.
- Use correlation IDs and event IDs for traceability.
- Monitor publish latency, subscriber lag, dead letters, dropped events, and schema compatibility.

## Pattern Boundaries and Common Confusions
- Competing Consumers distribute one message to one worker; Publisher-Subscriber fans out events.
- Choreography uses pub-sub to coordinate a process, but pub-sub alone is a communication pattern.
- Request-Reply is caller-specific; pub-sub is decoupled event distribution.

## Verification Checklist
- Can a new subscriber be added without publisher code changes?
- Can one subscriber fail while others continue receiving events?
- Are event schema changes compatible with existing subscribers?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
