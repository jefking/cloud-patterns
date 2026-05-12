# Queue-Based Load Leveling Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Queue-Based Load Leveling pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/queue-based-load-leveling
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Place a queue between producers and consumers so bursts are buffered and downstream processing happens at a sustainable rate.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Producers create intermittent load spikes that downstream services should not absorb directly.
- Work can be processed asynchronously after intake.
- Queue depth and message age can be monitored and used for scaling or backpressure.

Do not use this pattern when:
- The caller requires immediate completion of the work.
- The queue would hide overload without any drain capacity or backpressure plan.
- Messages cannot be made durable, retryable, or idempotent enough for asynchronous processing.

## Architecture Invariants
- A durable queue or equivalent buffer separates intake from processing.
- Producers enqueue work quickly; consumers drain according to capacity.
- Acknowledgement happens only after durable processing completion.
- Backlog health is visible through depth, age, throughput, and failure metrics.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define visibility timeout, retries, dead-letter handling, poison-message policy, and idempotency.
- Scale consumers based on queue age and downstream capacity.
- Apply backpressure or admission control when backlog exceeds safe limits.
- Test bursts, slow consumers, downstream outage, and recovery drain time.

## Pattern Boundaries and Common Confusions
- Competing Consumers scale workers; Queue-Based Load Leveling smooths producer demand.
- Asynchronous Request-Reply adds a status contract for a caller; this pattern may not expose per-request status.
- Priority Queue changes processing order; load leveling changes timing and buffering.

## Verification Checklist
- Can producers continue during short consumer slowdowns without direct overload?
- Can operators see when the queue is becoming unsafe?
- Are failed messages retried and isolated instead of blocking the whole queue?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
