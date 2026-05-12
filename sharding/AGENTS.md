# Sharding Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Sharding pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/sharding
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Horizontally partition data across shards and route requests to the shard that owns the relevant data.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- One data store cannot meet scale, availability, tenant isolation, or operational requirements alone.
- A shard key can distribute data and route requests predictably.
- The system can manage shard map, migrations, rebalancing, and cross-shard limitations.

Do not use this pattern when:
- The task is vertical table splitting, indexing, or caching only.
- Cross-shard transactions would be the normal write path.
- The shard key creates obvious hot shards and no mitigation exists.

## Architecture Invariants
- Every sharded entity has a deterministic shard placement rule.
- Business logic uses shard routing or a shard directory, not hardcoded locations.
- Per-shard schema, migrations, backup, monitoring, and access control are manageable.
- Cross-shard operations are minimized, explicit, and tested.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Plan shard split, merge, tenant move, resharding, and hot-shard mitigation.
- Monitor shard size, throughput, latency, errors, and skew.
- Handle shard directory consistency and failover.
- Document query patterns that cannot be served efficiently across shards.

## Pattern Boundaries and Common Confusions
- Index Table accelerates lookups; Sharding partitions storage ownership.
- Deployment Stamps partition deployments; Sharding partitions data.
- Bulkhead isolates failure and capacity; sharding may create isolation but data placement is central.

## Verification Checklist
- Can the system route any entity operation to the correct shard?
- Can a shard be added or rebalanced without code rewrites?
- Are cross-shard edge cases and hot shards tested?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
