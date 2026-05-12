# Compensating Transaction Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Compensating Transaction pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/compensating-transaction
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Recover from a failed eventually consistent multi-step operation by applying explicit compensating actions.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A business operation spans resources that cannot share one atomic transaction.
- Some completed steps must be undone, amended, or counteracted after later failure.
- The workflow can persist progress and safely run recovery actions.

Do not use this pattern when:
- A single local ACID transaction is available and sufficient.
- Completed actions are irreversible and no business compensation or reconciliation exists.
- The design relies only on in-memory progress to know what to compensate.

## Architecture Invariants
- Every forward step has a documented compensation, reconciliation, or explicit irreversible exception.
- Progress is durably recorded before or with each step so recovery can resume.
- Compensations are idempotent and safe to retry after partial failure.
- Compensation order follows business dependencies, not blind reverse-order assumptions.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Record step state, attempt count, timestamps, errors, and operator decisions.
- Define manual intervention paths for failed compensation.
- Use correlation IDs across all resources touched by the operation.
- Test crash recovery before, during, and after each compensation step.

## Pattern Boundaries and Common Confusions
- Saga is a broader transaction coordination pattern; compensating transaction is the recovery action strategy.
- Rollback is not always compensation; some business effects require counter-events or adjustments.
- Retries are not compensation unless they correct a previously committed effect.

## Verification Checklist
- Can the system list exactly which steps committed and which remain to compensate?
- Can compensation run twice without corrupting state?
- Are final states auditable and reconcilable?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
