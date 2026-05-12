# Static Content Hosting Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Static Content Hosting pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/static-content-hosting
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Serve static assets directly from storage or CDN infrastructure instead of application compute.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Assets are static or immutable enough to be served without application processing.
- Offloading delivery improves cost, latency, scalability, or availability.
- Access control, cache policy, and invalidation can be defined outside application request handling.

Do not use this pattern when:
- Assets require per-request business computation before delivery.
- Private content would be exposed publicly by storage or CDN configuration.
- The application server remains in the normal static-file data path.

## Architecture Invariants
- Static bytes are delivered by storage, object store, CDN, or equivalent platform service.
- Application compute is not required for normal asset reads.
- Cache headers, content types, versioning, and invalidation are explicit.
- Private assets use signed URLs, identity checks, or scoped access policies as needed.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Use content hashing, immutable filenames, or versioned paths for safe caching.
- Automate upload, metadata, compression, purge, and rollback.
- Monitor CDN hit rate, origin load, 404s, latency, and egress cost.
- Test direct delivery, cache refresh, and restricted access.

## Pattern Boundaries and Common Confusions
- Cache-Aside caches application data; Static Content Hosting serves assets directly.
- Valet Key may grant direct private asset access; this pattern is the hosting and delivery model.
- A web app serving files from its own process is not this pattern.

## Verification Checklist
- Can assets be fetched without invoking application servers?
- Are cache invalidation and versioning safe for updates?
- Are private assets inaccessible without the intended authorization mechanism?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
