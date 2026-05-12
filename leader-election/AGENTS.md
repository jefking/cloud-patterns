# Leader Election Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Leader Election pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/leader-election
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Elect exactly one active coordinator among distributed instances for leader-only work.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Several instances are capable of performing a task, but only one should do it at a time.
- The system has a distributed coordination mechanism such as leases, locks, consensus, or platform election.
- Failover to a new leader is required after leader failure.

Do not use this pattern when:
- A permanent hardcoded leader instance is acceptable.
- All instances can safely perform the work concurrently.
- No fencing, lease expiry, or stale-leader protection is possible.

## Architecture Invariants
- Leader-only work is protected by a lease, lock, epoch, term, or fencing token.
- Followers can detect who is leader or that no leader is currently valid.
- An old leader cannot keep writing after losing leadership.
- Leadership renewal, expiry, and transfer behavior are deterministic.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Account for clock skew, network partitions, slow leaders, and delayed storage writes.
- Expose current leader, term, renewal status, election attempts, and failover time.
- Prefer platform coordination primitives over hand-rolled ad hoc locks.
- Test crash, pause, partition, and stale-token scenarios.

## Pattern Boundaries and Common Confusions
- Scheduler Agent Supervisor coordinates workflows; Leader Election chooses a singleton owner.
- A leader is not necessarily a bottleneck for all work, only for protected coordination tasks.
- A database row flag is unsafe unless it has atomic compare-and-set, expiry, and fencing behavior.

## Verification Checklist
- Can two instances ever both perform leader-only writes?
- Can leadership move without manual intervention?
- Are stale leaders rejected by downstream resources?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
