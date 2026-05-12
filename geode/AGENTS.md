# Geode Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Geode pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/geodes
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Run active workload nodes across multiple geographies so any region can serve client requests.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- The workload needs active-active regional availability, low-latency global access, or regional outage tolerance.
- Each geographic node can serve live traffic, not merely wait as standby.
- Data replication, partitioning, and conflict handling can be designed explicitly.

Do not use this pattern when:
- The design is active-passive disaster recovery only.
- Every request still depends on a single central region for critical work.
- Data residency, consistency, or conflict requirements make active-active unsafe.

## Architecture Invariants
- Multiple geographic nodes are active and independently able to handle requests.
- Traffic steering selects a healthy and appropriate node based on latency, availability, or policy.
- Data replication and consistency rules are explicit for reads and writes.
- Regional health, failover, and re-entry are automated or operationally defined.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define conflict resolution, write ownership, replication lag, and regional data boundaries.
- Monitor per-region latency, error rates, replication health, and traffic distribution.
- Test regional isolation, failover, split-brain risk, and recovery after a region returns.
- Document which dependencies are global and could create shared fate.

## Pattern Boundaries and Common Confusions
- Deployment Stamps are repeatable units; Geode specifically emphasizes active geographic nodes.
- Sharding partitions data; Geode distributes service capability geographically.
- CDN static delivery is not Geode unless backend services are active across geographies.

## Verification Checklist
- Can at least two regions actively serve the same workload class?
- Can the workload survive loss of one region without central manual rewiring?
- Are consistency tradeoffs visible to callers and operators?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
