# Valet Key Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Valet Key pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/valet-key
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Issue a scoped, time-limited credential so a client can access a specific resource directly without broad service credentials.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- A client needs direct access to upload, download, or operate on a resource without proxying all bytes through the broker service.
- The broker can mint a constrained token or key for the exact resource and action.
- The target resource can enforce the delegated credential constraints.

Do not use this pattern when:
- The client receives a master key, broad account credential, or unrestricted token.
- The operation requires broker-side business validation on every byte transferred.
- Credentials are long-lived, reusable, or not bound to resource and action.

## Architecture Invariants
- The broker service authenticates and authorizes the client before issuing delegated access.
- The delegated credential is least-privilege, narrow-scope, and time-limited.
- The resource service enforces the credential without trusting client claims outside it.
- Credential issuance and use are auditable.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Bind credentials to tenant, user, resource, action, expiry, IP, content type, or size where appropriate.
- Define revocation, expiration, replay protection, and clock skew handling.
- Avoid logging full tokens or signed URLs.
- Test denied access for wrong action, wrong resource, expired token, and tampered token.

## Pattern Boundaries and Common Confusions
- Claim Check sends a payload reference through messaging; Valet Key delegates direct client access.
- Static Content Hosting may use valet keys for private assets, but Valet Key is the authorization delegation pattern.
- Federated Identity establishes identity; Valet Key delegates resource access after authorization.

## Verification Checklist
- Can the client perform only the intended operation on only the intended resource?
- Does access stop after expiry or revocation?
- Can the broker avoid handling the large data path while retaining control?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
