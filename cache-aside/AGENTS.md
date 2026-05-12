# Cache-Aside Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Cache-Aside pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/cache-aside
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Let application code populate a cache on demand from an authoritative data store and use it for later reads.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- The workload has frequently read data where cache hits improve latency, availability, or store load.
- The application can tolerate or control the consistency window between cache and source store.
- The primary data store remains authoritative for writes and recovery.

Do not use this pattern when:
- The cache would become the only durable source of truth.
- Data cannot be stale and no invalidation or refresh strategy is possible.
- The task is static asset hosting or CDN delivery rather than data lookup caching.

## Architecture Invariants
- Reads check the cache first, load from the store on miss, then populate the cache.
- Writes update the authoritative store and then invalidate, refresh, or expire affected cache entries.
- Cache keys, TTLs, serialization, and tenant scoping are deterministic.
- The application still works, with degraded performance, when the cache is empty or unavailable.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Protect expensive values from cache stampede with locks, single-flight, jittered TTLs, or background refresh.
- Set TTLs based on freshness requirements, not arbitrary defaults.
- Track hit rate, miss rate, evictions, stale reads, fill latency, and cache errors.
- Define invalidation for updates, deletes, permission changes, and schema changes.

## Pattern Boundaries and Common Confusions
- Materialized View precomputes a query model; Cache-Aside stores recently requested data on demand.
- Static Content Hosting serves immutable assets; Cache-Aside caches application data.
- Read-through cache products can implement this behavior, but application consistency rules still matter.

## Verification Checklist
- Do tests cover hit, miss, stale, invalidated, and unavailable-cache paths?
- Can a write never leave cache and store silently inconsistent?
- Are tenant and authorization boundaries encoded in cache keys or values?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
