# Choreography Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Choreography pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/choreography
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Coordinate a business process through events and local service reactions rather than a central orchestrator.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Multiple services participate in a process but can make local decisions from events.
- Loose coupling and independent service evolution are more valuable than centralized control flow.
- The process can tolerate eventual consistency and asynchronous propagation.

Do not use this pattern when:
- A central component must prescribe every step and branch of the process.
- The business process requires immediate ACID consistency across services.
- Services would coordinate through shared database state instead of events.

## Architecture Invariants
- Each service owns its local transaction and decides how to react to subscribed events.
- Publishers do not know which services subscribe or what they do.
- Events are versioned contracts that represent business facts or meaningful state changes.
- Handlers are idempotent and safe for duplicate, delayed, or out-of-order delivery.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Use correlation IDs and message history so the end-to-end process is traceable.
- Define dead-letter handling, retry budgets, and compensation for failed local reactions.
- Document event ownership, schema evolution, and subscription expectations.
- Monitor process-level progress without reintroducing command-and-control orchestration.

## Pattern Boundaries and Common Confusions
- Saga can be orchestrated or choreographed; Choreography is specifically decentralized event reaction.
- Publisher-Subscriber distributes events; Choreography uses those events to advance a business process.
- A synchronous call chain among services is not choreography.

## Verification Checklist
- Can a participant be added or changed without modifying publishers?
- Can the process be reconstructed from events and correlations?
- Are event duplicates, missing messages, and late arrivals explicitly handled?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
