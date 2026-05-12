# Claim Check Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Claim Check pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/claim-check
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns
- Additional background: https://www.enterpriseintegrationpatterns.com/patterns/messaging/

## Pattern Intent
Store a large or protected payload outside the message bus and send only a small reference through messaging.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Messages contain payloads too large, costly, sensitive, or slow for the broker path.
- Consumers can retrieve payloads from a separate store by using a controlled reference.
- Payload lifetime, permissions, and cleanup can be managed independently of message delivery.

Do not use this pattern when:
- The full payload is small, safe, and appropriate for the message transport.
- References would grant broad or long-lived access to protected data.
- The payload store cannot provide durability or access control appropriate for the data.

## Architecture Invariants
- The message contains metadata and a claim check reference, not the full payload.
- The payload store is authoritative for the externalized payload during the message workflow.
- References are scoped, time bounded when possible, and validated by consumers.
- Payload integrity, ownership, and content type are verifiable before use.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define cleanup for consumed, expired, failed, and orphaned payloads.
- Handle missing, expired, corrupted, or unauthorized claim checks as explicit failure states.
- Audit payload creation, access, and deletion separately from message processing.
- Keep correlation between message ID, payload ID, producer, and consumer.

## Pattern Boundaries and Common Confusions
- Valet Key delegates client access to a resource; Claim Check moves message payload data out of the broker.
- Messaging Bridge connects incompatible brokers; Claim Check changes message shape and storage.
- Do not use Claim Check to hide uncontrolled shared storage coupling.

## Verification Checklist
- Can the broker handle the message without the external payload bytes?
- Can unauthorized consumers fail to redeem the claim check?
- Are payload cleanup and orphan detection tested?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
