# Materialized View Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Materialized View pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/materialized-view
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Maintain a precomputed read model over one or more source stores for queries that are expensive or awkward against source data.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Read queries need a shape, join, aggregation, or denormalization that source stores cannot serve efficiently.
- A derived view can be refreshed within acceptable freshness bounds.
- Source stores remain authoritative and can rebuild the view.

Do not use this pattern when:
- A normal query, index, or cache is sufficient.
- The view would become the only durable source of truth.
- Consumers require perfectly current data and refresh lag cannot be tolerated.

## Architecture Invariants
- The view is derived, query-optimized, and rebuildable.
- Refresh triggers, schedule, or event handlers are explicit.
- Freshness and staleness are part of the contract.
- Writes do not treat the materialized view as authoritative business state.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define full rebuild, incremental refresh, failed refresh retry, and backfill behavior.
- Monitor view lag, refresh duration, failed updates, and query performance.
- Version view schema separately from source schema when needed.
- Protect readers during rebuilds with swap, versioned views, or consistent snapshots.

## Pattern Boundaries and Common Confusions
- Index Table optimizes lookups; Materialized View provides a precomputed query result shape.
- Cache-Aside fills on demand; Materialized View is maintained from source data.
- CQRS may use materialized views for reads, but the pattern here is the view itself.

## Verification Checklist
- Can the view be deleted and regenerated from sources?
- Do tests cover stale, refreshed, failed-refresh, and rebuilt view states?
- Is the intended query faster or simpler through the view?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
