# External Configuration Store Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the External Configuration Store pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/external-configuration-store
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Move configuration out of the application deployment package into a managed external source.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Configuration varies by environment, tenant, region, or runtime and should not require code changes.
- Multiple instances or services need consistent configuration management.
- The application can validate, cache, refresh, and fail safely when configuration changes.

Do not use this pattern when:
- The value is a secret that belongs in a secret manager, not ordinary configuration.
- The application cannot tolerate external configuration unavailability and has no safe fallback.
- Configuration changes would bypass review or create uncontrolled runtime behavior.

## Architecture Invariants
- Environment-specific values are outside packaged code and static binaries.
- Configuration source, precedence, schema, and ownership are explicit.
- The application validates required keys and allowed values before use.
- Secrets are separated and accessed through least-privilege secret mechanisms.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define refresh mode: startup-only, polling, push, or per-request cache with TTL.
- Audit configuration changes and support rollback to a known good version.
- Handle missing, malformed, stale, and unauthorized configuration predictably.
- Monitor configuration load failures and active configuration version.

## Pattern Boundaries and Common Confusions
- Feature flags can use this pattern but require rollout and targeting policy.
- Secret storage is related but not identical to configuration storage.
- Immutable infrastructure can still use external configuration if precedence and drift rules are clear.

## Verification Checklist
- Can routine environment changes happen without rebuilding the application package?
- Does invalid configuration fail fast or degrade safely?
- Can operators tell which configuration version an instance is using?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
