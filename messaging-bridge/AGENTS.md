# Messaging Bridge Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Messaging Bridge pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/messaging-bridge
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://www.enterpriseintegrationpatterns.com/patterns/messaging/MessagingBridge.html

## Pattern Intent
Connect otherwise incompatible messaging systems while preserving message intent and delivery semantics as far as possible.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Two messaging systems must exchange messages across incompatible protocols, formats, brokers, or routing models.
- A dedicated intermediary can translate and forward messages without owning domain business processing.
- Reliability expectations across both systems can be defined explicitly.

Do not use this pattern when:
- The task is normal application business processing inside a consumer.
- Both sides already use the same broker and contract with no compatibility gap.
- The bridge would silently discard required metadata or weaken delivery guarantees without disclosure.

## Architecture Invariants
- Source and target systems, channels, contracts, and acknowledgement semantics are named.
- Format, header, routing, identity, and correlation translations are explicit.
- The bridge does not add domain decisions beyond integration policy.
- Message loss, duplication, ordering, and poison-message behavior are defined.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Use durable checkpoints or transactions where available; otherwise document at-least-once or at-most-once risk.
- Prevent bridge loops with route markers, origin metadata, or topology rules.
- Monitor lag, retry count, dead-letter count, translation failures, and broker connectivity.
- Test target outage, source outage, malformed messages, and restart recovery.

## Pattern Boundaries and Common Confusions
- Anti-Corruption Layer translates domain semantics; Messaging Bridge translates messaging systems.
- Claim Check changes payload storage; Messaging Bridge changes transport compatibility.
- Publisher-Subscriber defines fan-out semantics, not broker interoperability by itself.

## Verification Checklist
- Can a message cross systems without losing required metadata?
- Is the bridge's delivery guarantee stated and tested?
- Can failures recover without uncontrolled duplicates or message loss?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
