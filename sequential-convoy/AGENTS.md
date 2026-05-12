# Sequential Convoy Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Sequential Convoy pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/sequential-convoy
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Process related messages in order for each group while allowing unrelated groups to proceed concurrently.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Messages have a correlation key, session, partition, or group whose order matters.
- Global serialization would unnecessarily block unrelated groups.
- The broker or processing layer can preserve per-group order.

Do not use this pattern when:
- All messages can be processed independently and out of order.
- Strict global order across all messages is required.
- No stable grouping key exists.

## Architecture Invariants
- The ordering group key is explicit and stable.
- Only one processor handles a given group at a time, or equivalent ordering control is enforced.
- Different groups can process concurrently when safe.
- Poison, missing, or delayed messages have a group-level recovery plan.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Use broker sessions, partitions, per-key locks, resequencing, or ordered streams as appropriate.
- Monitor blocked groups, group lag, poison messages, and ordering violations.
- Define how retries and dead letters affect group progress.
- Avoid hot groups by choosing keys and partitioning deliberately.

## Pattern Boundaries and Common Confusions
- Competing Consumers maximize parallelism; Sequential Convoy constrains parallelism within a group.
- Priority Queue reorders by importance; Sequential Convoy preserves required order.
- Kafka-style partitions can implement convoys but limit consumers by partition count.

## Verification Checklist
- Do tests prove same-group order under concurrency?
- Can other groups continue while one group is blocked?
- Is poison-message recovery scoped without breaking ordering guarantees?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
