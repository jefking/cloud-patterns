# Index Table Pattern Agents

## Scope
These instructions apply to every file in this folder. Build only software, examples, tests, and documentation that implement the Index Table pattern. Treat this file as the architectural contract for this subtree.

## Source Material
- Microsoft Learn pattern page: https://learn.microsoft.com/en-us/azure/architecture/patterns/index-table
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/architecture/patterns/
- Supporting architecture guidance: https://learn.microsoft.com/en-us/azure/well-architected/reliability/design-patterns

## Pattern Intent
Create and maintain secondary lookup structures for query patterns that the primary data store cannot serve efficiently.

## LLM Operating Contract
- First decide whether the requested work fits this pattern. If it does not fit, say so and propose the closest pattern folder instead of forcing the design.
- Keep the pattern as the primary architecture. Related patterns may support it only when their role is named and bounded.
- Prefer platform capabilities and well-known libraries over custom mechanisms when they preserve the pattern invariants.
- Make failure handling, observability, security, and operational ownership explicit. Do not leave them as TODOs in pattern-critical paths.
- Use precise names for pattern roles in code, diagrams, and explanations so reviewers can see the pattern boundary.
- When trade-offs are unavoidable, document the trade-off and the reason it is acceptable for this pattern.

## Applicability Gate
Use this pattern only when:
- Queries frequently filter or look up by fields that are not efficient primary keys or native indexes.
- A derived index table can map query keys to primary records or denormalized lookup data.
- The system can keep index entries synchronized, rebuilt, and repaired.

Do not use this pattern when:
- A native database index satisfies the query requirements without extra data modeling.
- The index would be treated as the authoritative record.
- The query is rare and does not justify maintenance complexity.

## Architecture Invariants
- The index exists for named query shapes and access patterns.
- Primary data remains the authority for business state.
- Index keys, values, and links to primary records are deterministic.
- Changes to indexed fields update, invalidate, or rebuild index entries.

## Implementation Requirements
- Identify the concrete participants, resources, and boundaries before writing code.
- Encode the pattern boundary in modules, interfaces, deployment units, configuration, or infrastructure, not only in comments.
- Make retries, timeouts, cancellation, idempotency, authorization, and correlation explicit where the pattern touches distributed calls or messages.
- Keep business rules in the component that owns the business capability; do not move them into infrastructure glue unless this pattern specifically calls for that ownership.
- Provide examples and tests that demonstrate the happy path, boundary enforcement, and at least one realistic failure path.

## Failure, Consistency, and Operations
- Define synchronous versus asynchronous index maintenance and acceptable lag.
- Provide rebuild and reconciliation jobs for stale, missing, or orphaned entries.
- Monitor index freshness, query performance, and repair counts.
- Handle deletes, rekeys, multi-tenant scoping, and uniqueness constraints.

## Pattern Boundaries and Common Confusions
- Materialized View stores a query-ready projection; Index Table stores lookup paths to records or small indexed data.
- Sharding routes data to partitions; Index Table accelerates lookup.
- Cache-Aside stores recently requested data; Index Table is a maintained access structure.

## Verification Checklist
- Does every index entry serve a documented query?
- Can the index be rebuilt from primary data?
- Do tests cover create, update, delete, and rekey behavior?
- Does the implementation still match the pattern if the technology choices are replaced?
- Are the operational signals sufficient for an on-call engineer to diagnose pattern-specific failure?
- Are tests focused on the pattern guarantees rather than only line coverage?

## Response Requirements For Agents
- In design responses, name the pattern roles and the reason this pattern is a fit.
- In code responses, point to the files that enforce the pattern boundary.
- If asked to add behavior that violates an invariant, stop and describe the conflict before changing files.
- If combining patterns, state which pattern is primary in this folder and which patterns are only supporting mechanisms.
