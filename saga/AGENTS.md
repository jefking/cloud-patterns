# Saga Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Saga pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/saga
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Manage consistency for a distributed business transaction through a sequence of local transactions and compensations.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A business transaction spans multiple services or data stores that cannot share one distributed ACID transaction.
- Each participant can commit a local transaction and expose a compensating action or reconciliation path.
- The workflow can persist saga state and correlation across participants.

Do not use this pattern when:
- A single service and database can handle the transaction atomically.
- Participants cannot compensate, reconcile, or tolerate eventual consistency.
- The design shares one database transaction across service boundaries.

## Architecture Invariants
- Every saga step has a local transaction and an outcome recorded in saga state.
- Every committed step has compensation, reconciliation, or an explicit irreversible business exception.
- Commands, events, and compensations are idempotent and correlated.
- The saga has terminal states such as completed, compensated, failed, or requiring manual intervention.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Choose orchestration or choreography deliberately and document who owns saga progress.
- Define timeouts, retries, dead letters, duplicate handling, and manual repair.
- Expose saga status, participant status, compensation attempts, and stuck workflows.
- Test every participant failure point and crash recovery after each committed local transaction.

## Pattern Boundaries and Common Confusions
- Compensating Transaction is the undo strategy; Saga is the distributed transaction workflow around it.
- Choreography can implement a saga, but a saga may also be orchestrated.
- Two-phase commit is not the normal saga solution.

## Verification Checklist
- Can the saga recover after coordinator or participant restart?
- Can duplicate messages not advance the saga twice?
- Can operators inspect and repair a stuck saga?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
