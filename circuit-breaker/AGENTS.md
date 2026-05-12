# Circuit Breaker Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Circuit Breaker pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://learn.microsoft.com/en-us/azure/architecture/microservices/design/patterns

## Pattern Intent
Stop repeated calls to a failing remote dependency, fail fast while it is unhealthy, and probe for recovery.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A remote dependency can fail or slow down for long enough that repeated calls waste resources.
- Callers need bounded latency, graceful degradation, or self-preservation under dependency failure.
- The system can define failure thresholds and recovery probes for the dependency.

Do not use this pattern when:
- The errors are deterministic validation or authorization failures, not dependency health signals.
- The dependency call is local and cannot create cascading resource exhaustion.
- The design retries forever or blocks callers while the dependency is known unhealthy.

## Architecture Invariants
- The breaker has closed, open, and half-open behavior or an equivalent state machine.
- Failure thresholds, timeout windows, reset timing, and probe limits are explicit.
- Open circuits reject or degrade calls before consuming downstream resources.
- Circuit state is scoped to one dependency or operation class with a shared failure profile.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Pair retries and circuit breakers under one total latency and attempt budget.
- Expose state changes, rejected calls, probe outcomes, dependency latency, and error rates.
- Provide fallback responses only when the business contract allows degraded data or behavior.
- Avoid synchronized recovery storms by limiting half-open probes.

## Pattern Boundaries and Common Confusions
- Retry assumes transient failure; Circuit Breaker assumes repeated attempts are currently harmful.
- Throttling protects a service from callers; Circuit Breaker protects callers and dependencies during failure.
- Health checks observe readiness; circuit breakers control live dependency calls.

## Verification Checklist
- Do tests force closed to open, open rejection, half-open probe, close on recovery, and reopen on failed probe?
- Can callers distinguish degraded behavior from normal success?
- Does opening the circuit measurably reduce pressure on the failing dependency?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
