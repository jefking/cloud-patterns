# Federated Identity Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Federated Identity pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/federated-identity
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Delegate authentication to an external identity provider and use validated claims for local authorization decisions.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Users, workloads, or organizations should authenticate through a trusted identity provider.
- The application should avoid owning credentials and authentication flows directly.
- External claims can be normalized into local authorization policy.

Do not use this pattern when:
- The application stores and verifies primary user passwords itself.
- Tokens are accepted without validating issuer, audience, signature, expiry, and required claims.
- External claims are treated as authorization without local policy.

## Architecture Invariants
- Authentication trust is delegated to a named identity provider.
- Token or assertion validation is strict and centralized.
- Local authorization maps normalized claims to permissions, roles, tenants, or scopes.
- Session lifetime, logout, refresh, and revocation behavior are explicit.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Plan key rotation, metadata discovery outages, clock skew, and token replay defenses.
- Log authentication failures without leaking tokens or personal data.
- Support multiple identity providers only with explicit issuer trust and claim normalization.
- Test disaster recovery assumptions when the identity provider is unavailable.

## Pattern Boundaries and Common Confusions
- Authentication proves identity; authorization decides allowed actions.
- API keys are not federated identity unless backed by a trusted identity assertion flow.
- Valet Key grants scoped resource access; Federated Identity establishes who the caller is.

## Verification Checklist
- Are invalid issuer, wrong audience, expired, unsigned, and malformed tokens rejected?
- Can claim mapping be tested independently from the identity provider?
- Do authorization tests prove least privilege for each role or scope?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
