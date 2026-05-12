# Quarantine Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Quarantine pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/quarantine
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Keep external assets isolated until they pass validation, scanning, approval, or quality gates.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- External or untrusted assets enter the workload and could harm security, quality, or operations.
- The workload can separate untrusted intake from trusted consumption paths.
- There are defined criteria for promotion, rejection, expiration, or manual review.

Do not use this pattern when:
- Unvalidated assets can be consumed directly by production workflows.
- The system cannot distinguish quarantined from trusted state.
- Validation outcome is based only on source claims, filenames, or client-provided flags.

## Architecture Invariants
- Every external asset starts in quarantine or an untrusted state.
- Trusted workloads consume only promoted assets.
- Promotion requires explicit successful validation or authorized review.
- Validation evidence and promotion decisions are auditable.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Scan for malware, schema validity, policy compliance, content safety, provenance, and size limits as appropriate.
- Define retention and cleanup for failed, expired, and abandoned assets.
- Prevent time-of-check/time-of-use swaps by binding validation to immutable content hashes or versions.
- Monitor validation failures, manual overrides, promotion latency, and scanner health.

## Pattern Boundaries and Common Confusions
- Valet Key grants limited access; Quarantine controls trust promotion.
- Claim Check externalizes payloads; Quarantine decides whether external assets are trusted.
- A staging folder is not quarantine unless production cannot consume from it before validation.

## Verification Checklist
- Can any unvalidated asset reach a trusted runtime path?
- Are rejected assets contained and auditable?
- Does promotion bind to the exact content that was validated?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
