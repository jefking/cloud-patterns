# Priority Queue Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Priority Queue pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/priority-queue
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Process higher-priority queued work before lower-priority work while preserving fairness rules the business requires.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Queued work has meaningful priority classes with different business urgency or service levels.
- Consumers can select, poll, or schedule work according to priority.
- The system can prevent or explicitly accept starvation of lower priorities.

Do not use this pattern when:
- All work has the same urgency and FIFO processing is sufficient.
- Producers can assign arbitrary priority without validation.
- Priority would bypass entitlement, tenant fairness, or security rules.

## Architecture Invariants
- Priority assignment rules are deterministic, validated, and auditable.
- Enqueue and dequeue behavior respects priority under load.
- Lower priorities have an explicit starvation, aging, quota, or reserved-capacity policy.
- Priority metadata cannot be forged by untrusted callers.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Implement with multiple queues, broker priority, sorted storage, or scheduler policy as appropriate.
- Track queue depth, age, throughput, and SLA by priority class.
- Test load with mixed priorities and downstream constraints.
- Document how retries affect priority and ordering.

## Pattern Boundaries and Common Confusions
- Queue-Based Load Leveling smooths bursts; Priority Queue orders queued work by importance.
- Throttling limits consumption; Priority Queue selects what to process first.
- Sequential Convoy preserves order by group; Priority Queue may intentionally reorder across priorities.

## Verification Checklist
- Does high-priority work complete sooner under contention?
- Can low-priority work make progress when required?
- Are priority rules enforced server-side or by trusted infrastructure?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
