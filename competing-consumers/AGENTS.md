# Competing Consumers Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Competing Consumers pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/competing-consumers
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://www.enterpriseintegrationpatterns.com/patterns/messaging/CompetingConsumers.html

## Pattern Intent
Run multiple interchangeable consumers against one work channel so each message is processed by one consumer while throughput scales out.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A queue or point-to-point channel receives more work than one consumer can process.
- Each work item should be handled once by any available consumer instance.
- The work can be made idempotent or protected against duplicate delivery.

Do not use this pattern when:
- Every subscriber must receive its own copy of each event; use Publisher-Subscriber.
- Strict global ordering is required for all messages.
- Consumers require unsafe shared memory or local state to coordinate work.

## Architecture Invariants
- Consumers are horizontally scalable replicas that pull or receive from the same logical work source.
- The broker or queue controls message ownership, lock, acknowledgement, and redelivery.
- A message is acknowledged only after durable completion of the work.
- Handlers tolerate duplicate delivery and crash after side effects.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define visibility timeout, lock renewal, retry count, and dead-letter behavior.
- Monitor queue depth, message age, consumer lag, processing time, duplicate rate, and poison messages.
- Scale consumers based on backlog and downstream capacity, not only CPU.
- For partitioned systems, document the relationship between partitions, ordering, and max parallelism.

## Pattern Boundaries and Common Confusions
- Publisher-Subscriber fans out copies; Competing Consumers distribute one work item to one consumer.
- Queue-Based Load Leveling buffers bursts; Competing Consumers scale processing of that buffer.
- Sequential Convoy preserves per-group ordering and may limit competition within a group.

## Verification Checklist
- Can one consumer crash without losing or duplicating unprotected work?
- Can adding consumers increase throughput until a real bottleneck is reached?
- Are poison messages isolated instead of blocking the whole channel?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
